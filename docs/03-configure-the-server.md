# Part 3 - Configure the server

> **Note:** **Copy-paste does not work** into the VM window (no VirtualBox Guest Additions on a minimal server, and you should not install them). Either type carefully, or connect with `ssh <login>@127.0.0.1 -p 4242` from a terminal on your host and paste there once SSH works.


## 3.1 Install sudo and give your user access

```bash
su -
apt update
apt install sudo vim
usermod -aG sudo <login>
getent group sudo
reboot
```

After the reboot, log in as `<login>` and test with `sudo -v`.


## 3.2 Check the hostname

```bash
hostnamectl
```

It must say `<login>42`. If not:

```bash
sudo hostnamectl set-hostname <login>42
sudo vim /etc/hosts
```

In `/etc/hosts` make the `127.0.1.1` line use the new hostname, then `sudo reboot`.


## 3.3 AppArmor

```bash
sudo aa-status
```

It must show AppArmor loaded. If the command is missing: `sudo apt install apparmor apparmor-utils`, then reboot.


## 3.4 SSH on port 4242, no root login

```bash
sudo apt install openssh-server
sudo vim /etc/ssh/sshd_config
```

Uncomment (remove the `#`) and set these two lines:

```bash
Port 4242
PermitRootLogin no
```

```bash
sudo systemctl restart ssh
sudo ss -tunlp | grep 4242
```

You should see `sshd` listening on port 4242 twice: once for IPv4 (`0.0.0.0`) and once for IPv6 (`[::]`). That is normal. If it is still on port 22, run `sudo systemctl disable --now ssh.socket && sudo systemctl enable --now ssh`.


## 3.5 Firewall (UFW)

```bash
sudo apt install ufw
sudo ufw allow 4242
sudo ufw enable
sudo ufw status verbose
```

Only port 4242 (v4 and v6) should be listed. Test from your host: `ssh <login>@127.0.0.1 -p 4242`.


## 3.6 Password aging

```bash
sudo vim /etc/login.defs
```

Set these three values:

```bash
PASS_MAX_DAYS 30
PASS_MIN_DAYS 2
PASS_WARN_AGE 7
```

These only affect **new** users. Apply them to the existing accounts too:

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

Change the `pam_pwquality.so` line to the following. **It is ONE single line** (it wraps here only because the page is narrow):

```bash
password requisite pam_pwquality.so retry=3 minlen=10 ucredit=-1 lcredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root
```

| Option | Meaning |
|---|---|
| `minlen=10` | at least 10 characters |
| `ucredit=-1` `lcredit=-1` `dcredit=-1` | at least one uppercase, one lowercase, one digit |
| `maxrepeat=3` | no more than 3 identical consecutive characters |
| `reject_username` | password must not contain the username |
| `difok=7` | at least 7 characters different from the old password |
| `enforce_for_root` | the policy also applies to root |


## 3.8 Strict sudo configuration

```bash
sudo mkdir -p /var/log/sudo
sudo visudo -f /etc/sudoers.d/sudo_config
```

Put these lines in the file. **Each `Defaults` line must be on ONE line.** A line broken in two is the classic cause of a `syntax error` in visudo:

```bash
Defaults passwd_tries=3
Defaults badpass_message="Wrong password, try again!"
Defaults logfile="/var/log/sudo/sudo.log"
Defaults log_input,log_output
Defaults iolog_dir="/var/log/sudo"
Defaults requiretty
Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```

Then verify the file and its permissions:

```bash
sudo chmod 0440 /etc/sudoers.d/sudo_config
sudo visudo -c
```

> **Note:** If `visudo` shows `What now?`, press `e` to edit again. **Never press `Q`** (it saves the broken file). Type the permissions as `0440` (four digits). `04440` (five digits) is a different, wrong value.


## 3.9 The user42 group

```bash
sudo groupadd user42
sudo usermod -aG user42 <login>
getent group user42
```


## 3.10 Change ALL passwords

The subject requires this **after** the configuration is in place, for your user and for root. Both new passwords must satisfy the policy.

```bash
passwd
sudo passwd root
```

---
[< Previous](./02-install-debian.md) | [Back to README](../README.md) | [Next >](./04-monitoring-and-cron.md)
