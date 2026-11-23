# Defense preparation

Be ready to explain and demonstrate:

- Why Debian, and how it differs from Rocky
- `apt` vs `aptitude`, and what AppArmor is
- What LVM is and why the disk is encrypted
- How SSH, UFW, sudo and the password policy work
- How cron works and how your script collects each value
- Live: create a user and group, change the hostname, open and close a firewall port, stop the monitoring script


## Concept questions

Short answers to check your understanding. The evaluator will ask follow-ups, so understand them rather than memorize them.

**Virtual machine / hypervisor?**
A VM is a software-emulated computer with its own virtual CPU, memory, disk and network. The hypervisor (VirtualBox) creates VMs and shares the host's hardware between them.

**Debian vs Rocky?**
Debian: community-driven, `apt`/`dpkg`, AppArmor, UFW. Rocky: Red Hat Enterprise Linux compatible, `dnf`/`rpm`, SELinux, firewalld.

**`apt` vs `aptitude`?**
Both sit on top of `dpkg` and resolve dependencies. `apt` is the standard command-line tool. `aptitude` has a text interface and a more elaborate dependency solver.

**AppArmor?**
Mandatory Access Control: each program is confined by a profile. *Enforce* mode blocks violations, *complain* mode only logs them. Check with `sudo aa-status`.

**LVM?**
PV (disk/partition) > VG (pool) > LV (what you mount). LVs can be resized easily. Here it runs inside an encrypted (LUKS) partition.

**SSH, and why no root login?**
Encrypted remote shell. Root is the most targeted account, so you log in as a normal user and use `sudo`. Port 4242 avoids default-port scans.

**UFW?**
A simple front-end to the kernel firewall. `sudo ufw allow 4242`, `sudo ufw status`.

**sudo?**
Runs commands as root and logs them. The strict rules limit attempts and make actions auditable.

**cron and wall?**
`cron` runs commands on a schedule. `wall` broadcasts a message to all terminals.

## Live tasks to practice

| Task | Command |
|---|---|
| Show OS | `cat /etc/os-release` |
| Partitions / LVM | `lsblk` |
| Firewall | `sudo ufw status` |
| SSH status | `sudo systemctl status ssh` |
| AppArmor | `sudo aa-status` |
| New user + group | `sudo adduser <name>`, `sudo usermod -aG user42 <name>` |
| User password rules | `sudo chage -l <name>` |
| Change hostname | `sudo hostnamectl set-hostname <new>`, edit `/etc/hosts`, `sudo reboot` |
| Open / close a port | `sudo ufw allow <port>`, `sudo ufw delete allow <port>` |
| Stop the broadcast | comment the line in `sudo crontab -u root -e` |

---
[< Previous](./06-troubleshooting.md) | [Back to README](../README.md)
