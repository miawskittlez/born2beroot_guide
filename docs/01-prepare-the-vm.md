# Part 1 - Prepare the host and create the VM


## 1.1 Download the Debian ISO

Go to **https://www.debian.org/download** and download the **64-bit PC netinst** ISO of the current stable release.


## 1.2 Choose where the VM is stored

- Do **not** keep the VM in your home folder (small quota) or in a temporary folder that is wiped.
- At many 42 campuses `goinfre` is local and wiped regularly (often at logout), while `sgoinfre` is shared and persistent. **Rules differ per campus: confirm with staff or your campus docs.**
- Test it: create a small file there, log out, log back in, and check that it is still there.
- Whatever you choose, **back up the VM folder** to an external drive once it works.


## 1.3 Create the VM in VirtualBox

1. Click **New**. Name: `<login>42`. Folder: your persistent storage folder. ISO: the Debian ISO. Type Linux, version Debian (64-bit).
2. **Tick "Skip Unattended Installation"**, otherwise VirtualBox installs Debian for you and you learn nothing (and may break the rules).
3. Hardware: 1024 MB RAM, 1-2 CPUs.
4. Hard disk: a **VDI**, **dynamically allocated**. 12 GB is enough for the mandatory part; use about 30.8 GB if you want to try the bonus later.


## 1.4 Port forwarding for SSH

This lets your host machine reach SSH inside the VM. The VM must use **NAT** networking (the default).

1. Select the VM, click **Settings &gt; Network &gt; Adapter 1** and check "Attached to: NAT".
2. Click **Advanced &gt; Port Forwarding**, then the green **+** button.
3. Fill in one row exactly like the table below, then click OK twice.

| Name | Protocol | Host IP | Host Port | Guest IP | Guest Port |
|---|---|---|---|---|---|
| ssh | TCP | 127.0.0.1 | 4242 | 10.0.2.15 | 4242 |

> **Note:** Leaving the IP columns blank often causes `kex_exchange_identification: Connection closed by remote host`. Fill them in. The guest IP is normally `10.0.2.15` (check with `ip a` inside the VM).

---
[Back to README](../README.md) | [Next >](./02-install-debian.md)
