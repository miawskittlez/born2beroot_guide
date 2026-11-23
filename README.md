# Born2beRoot - Step-by-step guide (Debian)

From an empty VirtualBox to a signed submission. Also available as one file: [`Born2beRoot_Guide.pdf`](Born2beRoot_Guide.pdf).

> - Replace `<login>` with your 42 login (login `wil` gives hostname `wil42`).
> - Targets **Debian 13 "trixie"**. Your subject PDF is always the reference.
> - `monitoring.sh` is **not included**: you write it yourself ([Part 4](docs/04-monitoring-and-cron.md)).

## Contents

1. [Prepare the host and create the VM](docs/01-prepare-the-vm.md)
2. [Install Debian](docs/02-install-debian.md)
3. [Configure the server](docs/03-configure-the-server.md)
4. [Monitoring script and cron](docs/04-monitoring-and-cron.md)
5. [Final checks and signature](docs/05-final-checks-and-signature.md)
6. [Troubleshooting](docs/06-troubleshooting.md)
7. [Defense preparation and Q&A](docs/07-defense-qa.md)
8. [Cheat sheet: vim and nano](docs/08-cheatsheet-vim-nano.md)

## Checklist

- [ ] Debian ISO downloaded, VM created, port forwarding set
- [ ] Debian installed with encrypted LVM and no desktop
- [ ] sudo installed, user in `sudo`
- [ ] Hostname `<login>42`
- [ ] AppArmor running
- [ ] SSH on 4242, no root login
- [ ] UFW on, only 4242 open
- [ ] Password aging and strength policy
- [ ] sudo rules and logs
- [ ] Group `user42`
- [ ] All passwords changed
- [ ] `monitoring.sh` written, runs every 10 minutes
- [ ] Final checks, shut down, `signature.txt` pushed

## Key concepts

- **VM**: software computer; the **hypervisor** (VirtualBox) shares the host's hardware between guests.
- **LVM**: PV (disk) > VG (pool) > LV (what you mount), resizable, here on an encrypted partition.
- **AppArmor**: per-program profiles that limit what a program may access.
- **apt / aptitude / dpkg**: `dpkg` installs one package; `apt` and `aptitude` also resolve dependencies.
- **SSH**: encrypted remote shell. **UFW**: simple firewall. **sudo**: root commands, logged.
- **cron / wall**: scheduler / broadcast to all terminals.

More in [Defense Q&A](docs/07-defense-qa.md).

## Requirements at a glance

| Area | Requirement |
|---|---|
| OS | Latest stable Debian, **no graphical interface** |
| Disk | At least 2 **encrypted** partitions with **LVM** |
| Hostname | `<login>42` |
| Users | `<login>` in the `sudo` and `user42` groups |
| SSH | Port **4242**, **no root login** |
| Firewall | UFW on, **only port 4242** open |
| AppArmor | Running at startup |
| Passwords | Expire after 30 days, min 2 days between changes, warning 7 days before; min 10 chars, upper + lower + digit, max 3 identical in a row, no username, 7+ chars different from the old one (not root); applies to root too |
| sudo | 3 attempts, custom error message, I/O logs in `/var/log/sudo/`, TTY on, restricted `secure_path` |
| Monitoring | `monitoring.sh` broadcast to all terminals every 10 minutes |
| Submission | `signature.txt` with the SHA-1 of the `.vdi` disk |


The optional bonus part is not covered.
