# Part 2 - Install Debian

Start the VM and choose **Install** (the text installer, NOT "Graphical install"). Keys: arrows to move, **Enter** to select, **Space** to tick a box, **Tab** to jump to buttons like &lt;Continue&gt; and &lt;Go Back&gt;.


## 2.1 Basic questions

- Language, location and keyboard: choose what matches your keyboard. (A wrong layout later makes characters like `/` and `?` come out wrong.)
- **Hostname: `<login>42`.** The installer pre-fills the word `debian`; **delete it first**, or your hostname will be `debian<login>42`.
- Domain name: leave blank.
- Root password and your user's password: strong passwords that already satisfy the policy (10+ chars, upper, lower, digit, no 3 identical in a row, not containing the username).
- Full name and username: your login.


## 2.2 Partitioning (the hard part)

Choose **Manual**. The target layout looks like this:

```text
sda
 |- sda1   ~500 MB    /boot            (NOT encrypted)
 |- sda2   1 KB       (extended partition, created automatically)
 `- sda5   the rest   encrypted
     `- LVM volume group  <login>42-vg
         |- root   /
         |- swap   [SWAP]
         `- home   /home
```

| Logical volume | Size for ~30 GB disk | Size for 12 GB disk |
|---|---|---|
| root | 10G | 6G |
| swap | 2G | 1G |
| home | 5G (or max) | the rest |

Sizes are your choice; the subject's example uses arbitrary sizes. Pick sizes that work without wasting disk.

1. Select the disk (`sda`) and create a **new empty partition table**. If it asks for the type, choose `msdos`.
2. Select FREE SPACE &gt; **Create a new partition**. Size `500MB`, type **Primary**, location **Beginning**. Use as **Ext4**, mount point **/boot**. Done setting up the partition.
3. Select the remaining FREE SPACE &gt; Create a new partition. Size `max`, type **Logical**. **Use as: physical volume for encryption**. Done setting up the partition.
4. At the top, choose **Configure encrypted volumes** &gt; Yes &gt; **Create encrypted volumes**. Tick the **big** partition (`/dev/sda5`) with Space, NOT the 500 MB one. Finish.
5. The installer now erases the partition. This can take a long time. It is a security nicety that does not matter on a brand-new virtual disk, so you may **cancel it** (Tab to &lt;Cancel&gt;) if you do not want to wait.
6. Enter an **encryption passphrase** twice. **Write it down.** You type it at every boot and cannot recover the VM without it.
7. Choose **Configure the Logical Volume Manager** &gt; Yes. **Create volume group** named `<login>42-vg` and tick `/dev/mapper/sda5_crypt`.
8. **Create logical volume** three times (`root`, `swap`, `home`) using the sizes above.
9. Finish, then assign each volume: `root` = Ext4 on `/`, `home` = Ext4 on `/home`, `swap` = swap area. Confirm `sda1` is still Ext4 on `/boot`.
10. **Finish partitioning and write changes to disk** &gt; Yes.

> **Note:** If the installer does NOT ask for a table type or Primary/Logical, you probably got a GPT table. That is fine. In BIOS mode (VirtualBox default, EFI unticked) first create a **1 MB partition used as "Reserved BIOS boot area"**, then the 500 MB `/boot`, then the big encrypted partition. In EFI mode create a 500 MB "EFI System Partition" first instead. The encrypted device will then be `sda3` instead of `sda5`.


## 2.2b Finish the installation

1. "Scan extra installation media?" &gt; **No**.
2. Mirror: choose your country and `deb.debian.org`. Proxy: blank.
3. Package usage survey: No.
4. **Software selection: untick EVERYTHING** (no desktop environment, no SSH server). Keep only **standard system utilities**.
5. "Install the GRUB boot loader to your primary drive?" &gt; **Yes**, then select `/dev/sda`.
6. When installation is complete, continue to reboot. Enter the encryption passphrase at boot, then log in as `<login>`.

---
[< Previous](./01-prepare-the-vm.md) | [Back to README](../README.md) | [Next >](./03-configure-the-server.md)
