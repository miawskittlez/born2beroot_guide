# Born2beRoot - Step-by-step guide (Debian)

A beginner-friendly walkthrough for the 42 project **Born2beRoot**, from an empty VirtualBox to a signed submission.
Written from a real first-time walkthrough, so it includes the mistakes people actually make and how to fix them.

> **About `monitoring.sh`:** it is **not** included. It is a graded part of the project and you must write it and explain it at the defense. [Part 4](docs/04-monitoring-and-cron.md) lists exactly what it must show and how to schedule it.
>
> **Always follow your own subject PDF.** This guide targets **Debian 13 "trixie"** (stable at the time of writing, November 2026). Rules and campus setups can differ.

Everywhere you see `<login>`, use your own 42 login (for example, login `wil` gives hostname `wil42`).

A single-file version of everything is available as [`Born2beRoot_Guide.pdf`](Born2beRoot_Guide.pdf).

## Contents

1. [Prepare the host and create the VM](docs/01-prepare-the-vm.md)
2. [Install Debian (encrypted LVM partitioning)](docs/02-install-debian.md)
3. [Configure the server (sudo, SSH, UFW, password policy)](docs/03-configure-the-server.md)
4. [The monitoring script and cron](docs/04-monitoring-and-cron.md)
5. [Final checks and signature](docs/05-final-checks-and-signature.md)
6. [Troubleshooting](docs/06-troubleshooting.md)
7. [Defense preparation and Q&A](docs/07-defense-qa.md)
8. [Cheat sheet: vim and nano](docs/08-cheatsheet-vim-nano.md)

## Roadmap checklist

- [ ] Download the Debian netinst ISO
- [ ] Create the VM (skip unattended install) and set up port forwarding
- [ ] Install Debian with encrypted LVM, no desktop environment
- [ ] Install sudo, add your user to `sudo`
- [ ] Hostname is `<login>42`
- [ ] AppArmor running
- [ ] SSH on port 4242, root login disabled
- [ ] UFW enabled, only port 4242 open
- [ ] Password aging and password strength policy
- [ ] Strict sudo configuration and logs
- [ ] Group `user42` and your user inside it
- [ ] All passwords changed to comply with the policy
- [ ] `monitoring.sh` written and scheduled every 10 minutes
- [ ] Final checks, shut down, `signature.txt` pushed

## Concepts you should understand

**Virtual machines.** A virtual machine is software that behaves like a real computer, with its own virtual CPU, memory, disk and network. A *hypervisor* (VirtualBox here) creates VMs and shares the *host's* hardware among the *guests* while keeping them isolated. This lets you run a different operating system on the same machine and test things safely.

**LVM and encryption.** LVM sits between your disk and your file systems. A *physical volume* (PV) is a disk or partition, a *volume group* (VG) pools PVs, and *logical volumes* (LV) are carved out of the VG for `/`, swap, `/home`, and so on. LVs can be resized easily. In this project the LVM sits inside an encrypted (LUKS) partition, so the data is protected at rest.

**AppArmor.** A Linux security module for Mandatory Access Control. Each program can be confined by a profile that limits what files and resources it may use. In *enforce* mode violations are blocked, in *complain* mode they are only logged.

**`apt`, `aptitude` and `dpkg`.** `dpkg` installs individual `.deb` files without resolving dependencies. `apt` builds on it and resolves dependencies automatically. `aptitude` is a higher-level front-end with a text interface and a more elaborate dependency solver.

**SSH.** An encrypted protocol for remote shell access, replacing unencrypted tools like telnet. You connect with `ssh <user>@<host> -p <port>`.

**UFW.** A simple front-end to the kernel firewall. You allow or deny ports with commands like `sudo ufw allow 4242`.

**`sudo`.** Lets permitted users run commands as root and records what they did. The strict configuration in this project limits attempts and logs everything.

**`cron` and `wall`.** `cron` runs commands on a schedule. `wall` broadcasts a message to every logged-in terminal. Together they produce the monitoring banner every 10 minutes.

More detail and sample defense questions are in [docs/07-defense-qa.md](docs/07-defense-qa.md).

## Requirements at a glance

| Area | What the subject asks for |
|---|---|
| OS | Latest stable Debian (or Rocky). **No graphical interface** (no X.org, Wayland, etc.), otherwise grade 0. |
| Disk | At least 2 **encrypted** partitions using **LVM** |
| Hostname | `<login>42` (you will change it during the defense) |
| Users | root plus a user named `<login>`, belonging to the `sudo` and `user42` groups |
| SSH | Running on **port 4242**, **root login disabled** |
| Firewall | UFW active at boot, **only port 4242** open |
| Mandatory access control | AppArmor running at startup (Debian) |
| Password policy | Expires every 30 days, min 2 days between changes, warning 7 days before expiry; min 10 chars, upper + lower + digit, max 3 identical consecutive chars, must not contain the username, at least 7 chars different from the old password (not for root); root must comply too |
| sudo | 3 attempts max, custom wrong-password message, log inputs and outputs in `/var/log/sudo/`, TTY mode on, restricted `secure_path` |
| Monitoring | `monitoring.sh` in bash, broadcast to all terminals every 10 minutes |
| Submission | `signature.txt` with the SHA-1 of your VM's `.vdi` disk |


## Not covered

The optional **bonus** part (web server, database, extra service) is not covered here.

## Found a mistake?

Open an issue or a pull request. Corrections are welcome, especially if the subject or Debian changes.
