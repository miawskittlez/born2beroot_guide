# Part 1 - Prepare the host and create the VM


## 1.1 Download Debian

Download the **64-bit netinst ISO** from **https://www.debian.org/download**.


## 1.2 Choose where to store the VM

- Use persistent storage, not your home folder (small quota). At many campuses that is `sgoinfre`; `goinfre` is usually wiped. Check your campus rules.
- Back up the VM folder to an external drive once it works.


## 1.3 Create the VM

1. VirtualBox &gt; **New**. Name `<login>42`, folder = your storage folder, ISO = the Debian ISO, Linux / Debian (64-bit).
2. Tick **Skip Unattended Installation**.
3. 1024 MB RAM, 1-2 CPUs.
4. Disk: **VDI**, **dynamically allocated**, 12 GB (about 30.8 GB if you plan the bonus).


## 1.4 Port forwarding for SSH

1. Select the VM &gt; **Settings &gt; Network &gt; Adapter 1**. It must be **NAT**.
2. **Advanced &gt; Port Forwarding** &gt; green **+**, then fill in this row.

| Name | Protocol | Host IP | Host Port | Guest IP | Guest Port |
|---|---|---|---|---|---|
| ssh | TCP | 127.0.0.1 | 4242 | 10.0.2.15 | 4242 |

---
[Back to README](../README.md) | [Next >](./02-install-debian.md)
