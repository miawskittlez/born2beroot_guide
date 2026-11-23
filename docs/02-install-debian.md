# Part 2 - Install Debian

Start the VM and choose **Install** (not Graphical install). Keys: arrows to move, **Enter** to select, **Space** to tick, **Tab** to reach buttons.


## 2.1 Basic questions

- Language, location, keyboard: pick yours.
- Hostname: `<login>42` (clear any pre-filled text first). Domain: blank.
- Root password and user password: strong, and already compliant with the policy.
- Full name / username: your login.


## 2.2 Partitioning

Choose **Manual**. Target layout:

```text
sda
 |- sda1   500 MB   /boot         (not encrypted)
 `- sda5   the rest (encrypted)
     `- LVM group <login>42-vg
         |- root   /
         |- swap   [SWAP]
         `- home   /home
```

| Logical volume | 30 GB disk | 12 GB disk |
|---|---|---|
| root | 10G | 6G |
| swap | 2G | 1G |
| home | 5G | rest |

1. Select the disk and create a **new empty partition table** (`msdos` if asked).
2. FREE SPACE &gt; **Create a new partition**: `500MB`, **Primary**, **Beginning**, Ext4, mount point **/boot**.
3. FREE SPACE &gt; new partition: `max`, **Logical**, use as **physical volume for encryption**.
4. **Configure encrypted volumes** &gt; Yes &gt; **Create encrypted volumes**. Tick the **big** partition only (`sda5`). Finish.
5. The disk erase can be slow. You can cancel it (Tab to &lt;Cancel&gt;); a new virtual disk has nothing to wipe.
6. Enter an **encryption passphrase** twice and **write it down**.
7. **Configure the Logical Volume Manager** &gt; Yes &gt; **Create volume group** `<login>42-vg`, tick `sda5_crypt`.
8. Create 3 logical volumes: `root`, `swap`, `home` (sizes above).
9. Finish. Set `root` = Ext4 on `/`, `home` = Ext4 on `/home`, `swap` = swap area.
10. **Finish partitioning and write changes to disk** &gt; Yes.

> **Note:** No Primary/Logical question? You have a GPT table, which works too. In BIOS mode create a **1 MB "Reserved BIOS boot area"** partition first, then the 500 MB `/boot`, then the encrypted one (it will be `sda3`). In EFI mode make a 500 MB "EFI System Partition" first.


## 2.3 Finish the installation

1. "Scan extra installation media?" &gt; **No**.
2. Mirror: your country, `deb.debian.org`, no proxy. Survey: No.
3. **Software selection: untick everything** (no desktop, no SSH server). Keep only **standard system utilities**.
4. Install GRUB &gt; **Yes** &gt; `/dev/sda`.
5. Reboot, enter the encryption passphrase, log in as `<login>`.

---
[< Previous](./01-prepare-the-vm.md) | [Back to README](../README.md) | [Next >](./03-configure-the-server.md)
