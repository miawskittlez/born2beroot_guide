# Part 3 - Configure the server

> **Note:** You can't paste into the VM window. Type commands, or connect from your host with `ssh <login>@127.0.0.1 -p 4242` once SSH works and paste there.


## 3.1 sudo

```bash
su -
apt update
apt install sudo vim
usermod -aG sudo <login>
getent group sudo
reboot
```

`getent group sudo` should list `<login>`. After the reboot, log in again and test with `sudo -v`.


## 3.2 Hostname

```bash
hostnamectl
```

It must show `<login>42`. To change it:

```bash
sudo hostnamectl set-hostname <login>42
sudo vim /etc/hosts
```

Update the `127.0.1.1` line to the new name, then `sudo reboot`.


## 3.3 AppArmor

```bash
sudo aa-status
```

It must show AppArmor loaded. If missing: `sudo apt install apparmor apparmor-utils`, then reboot.


## 3.4 SSH

```bash
sudo apt install openssh-server
sudo vim /etc/ssh/sshd_config
```

Uncomment and set:

```bash
Port 4242
PermitRootLogin no
```

```bash
sudo systemctl restart ssh
sudo ss -tunlp | grep 4242
```

`sshd` should appear on port 4242 for IPv4 and IPv6. If it is still on 22: `sudo systemctl disable --now ssh.socket && sudo systemctl enable --now ssh`.


## 3.5 Firewall

```bash
sudo apt install ufw
sudo ufw allow 4242
sudo ufw enable
sudo ufw status verbose
```

Only 4242 (v4 and v6) should be listed. Test from your host: `ssh <login>@127.0.0.1 -p 4242`.


## 3.6 Password aging

```bash
sudo vim /etc/login.defs
```

```bash
PASS_MAX_DAYS 30
PASS_MIN_DAYS 2
PASS_WARN_AGE 7
```

This only affects new users, so also apply it to existing accounts:

```bash
sudo chage -M 30 -m 2 -W 7 <login>
sudo chage -M 30 -m 2 -W 7 root
sudo chage -l <login>
```


## 3.7 Password strength

```bash
sudo apt install libpam-pwquality
sudo vim /etc/pam.d/common-password
```

Make the `pam_pwquality.so` line this (**one single line**):

```bash
password requisite pam_pwquality.so retry=3 minlen=10 ucredit=-1 lcredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root
```

| Option | Meaning |
|---|---|
| `minlen=10` | at least 10 characters |
| `ucredit=-1 lcredit=-1 dcredit=-1` | one uppercase, one lowercase, one digit |
| `maxrepeat=3` | max 3 identical characters in a row |
| `reject_username` | no username in the password |
| `difok=7` | 7 characters different from the old password |
| `enforce_for_root` | applies to root too |


## 3.8 sudo rules

```bash
sudo mkdir -p /var/log/sudo
sudo visudo -f /etc/sudoers.d/sudo_config
```

File contents (**each line on one line**):

```bash
Defaults passwd_tries=3
Defaults badpass_message="Wrong password, try again!"
Defaults logfile="/var/log/sudo/sudo.log"
Defaults log_input,log_output
Defaults iolog_dir="/var/log/sudo"
Defaults requiretty
Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```

```bash
sudo chmod 0440 /etc/sudoers.d/sudo_config
sudo visudo -c
```

If `visudo` shows `What now?`, press `e` to fix the file. Never press `Q`.


## 3.9 The user42 group

```bash
sudo groupadd user42
sudo usermod -aG user42 <login>
getent group user42
```


## 3.10 Change all passwords

Do this last, for your user and root. Both must meet the policy.

```bash
passwd
sudo passwd root
```

---
[< Previous](./02-install-debian.md) | [Back to README](../README.md) | [Next >](./04-monitoring-and-cron.md)
