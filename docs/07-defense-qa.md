# Defense preparation

You will be asked to explain and demonstrate. Make sure you can answer in your own words:

- Why Debian (or Rocky), and how they differ
- `apt` vs `aptitude`; what AppArmor (or SELinux) is
- What LVM is (physical volume, volume group, logical volume) and why the disk is encrypted
- How SSH works, why root login is disabled, and why the port is 4242
- How UFW works and how to open or close a port
- How sudo is configured, and what each line of your sudoers file does
- How the password policy is enforced
- How cron works, how your script collects each value, and how to stop it without editing it
- Live tasks: create a new user and assign it to a group, change the hostname, add/remove a firewall rule


## Concept questions (with short answers in my own words)

Use these to check your understanding. Do not memorize them word for word: the evaluator will ask follow-up questions.

**What is a virtual machine and what is a hypervisor?**
A VM is a software-emulated computer with its own virtual CPU, memory, disk and network that runs its own operating system. The hypervisor (here VirtualBox) is the program that creates VMs and shares the host's real hardware between them while keeping them isolated.

**Why did you choose Debian, and how does it differ from Rocky?**
Debian is a community-driven, stable distribution that is friendly to beginners and widely documented. Rocky is a community rebuild that is compatible with Red Hat Enterprise Linux. In practice: Debian uses `apt`/`dpkg` (`.deb` packages), AppArmor and UFW; Rocky uses `dnf`/`rpm` (`.rpm` packages), SELinux and firewalld.

**What is the difference between `apt` and `aptitude`?**
Both manage packages on top of `dpkg`, which installs single `.deb` files but does not resolve dependencies. `apt` is the standard command-line tool that resolves and downloads dependencies. `aptitude` is a higher-level front-end with a text interface and a more elaborate dependency solver that can propose alternative solutions when there is a conflict.

**What is AppArmor?**
A Linux security module that provides Mandatory Access Control. Each program can be confined by a profile that lists which files, capabilities and network resources it may use. In *enforce* mode violations are blocked; in *complain* mode they are only logged. Check it with `sudo aa-status`.

**What is LVM and why use it?**
LVM is a layer between the disks and the file systems. A **physical volume** (PV) is a disk or partition, a **volume group** (VG) pools one or more PVs, and **logical volumes** (LVs) are carved out of the VG and hold file systems or swap. The benefit is flexibility: you can resize LVs without worrying about contiguous disk space. In this project the LVM sits on top of an **encrypted** partition (LUKS), so the data on disk is protected without a passphrase.

**What is SSH and why disable root login?**
SSH is an encrypted protocol for remote shell access. Root is the most targeted account, so forbidding it forces attackers to guess a username as well as a password, and gives you accountability through named users plus `sudo`. The port is changed to 4242 to reduce automated scans on the default port 22.

**What is UFW?**
"Uncomplicated Firewall", a simple front-end for the kernel firewall (iptables/nftables). You set rules such as `sudo ufw allow 4242` and look at them with `sudo ufw status`.

**What is `sudo` and why configure it strictly?**
`sudo` lets permitted users run commands with root privileges and records them. The strict rules (limited attempts, TTY required, restricted `secure_path`, logs of inputs and outputs) reduce abuse and make actions auditable.

**What is `cron`? What is `wall`?**
`cron` is the scheduler that runs commands at fixed times (here, every 10 minutes through root's crontab). `wall` sends a message to every logged-in terminal.

**How does the password policy work?**
Aging settings (`/etc/login.defs`, `chage`) control expiry; strength rules come from `libpam-pwquality` options in `/etc/pam.d/common-password`.

## Things the evaluator may ask you to do live

| Task | How |
|---|---|
| Show the OS and that there is no GUI | `cat /etc/os-release`, `ls /usr/bin/*session` (should find nothing graphical) |
| Check partitions and LVM | `lsblk` |
| Check UFW | `sudo ufw status` |
| Check SSH | `sudo systemctl status ssh`, then connect with `ssh <login>@127.0.0.1 -p 4242` |
| Check AppArmor | `sudo aa-status` |
| Create a user and assign a group | `sudo adduser <name>`, `sudo usermod -aG user42 <name>`, `getent group user42` |
| Check the new user's password rules | `sudo chage -l <name>` |
| Change the hostname | `sudo hostnamectl set-hostname <new>`, edit `/etc/hosts`, `sudo reboot` |
| Open or close a port | `sudo ufw allow <port>`, `sudo ufw delete allow <port>` |
| Show the sudo log | `sudo ls /var/log/sudo/` |
| Stop the monitoring broadcast without editing the script | comment the line in `sudo crontab -u root -e`, or `sudo systemctl stop cron` |
| Explain the script | walk through it line by line |

> Verify these on your own machine before the defense, because the exact tasks depend on your evaluator.

---
[< Previous](./06-troubleshooting.md) | [Back to README](../README.md) | [Next >](./08-cheatsheet-vim-nano.md)
