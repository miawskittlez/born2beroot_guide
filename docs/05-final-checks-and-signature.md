# Part 5 - Final checks

```bash
hostnamectl                        # Static hostname: <login>42
lsblk                              # /boot, sda5_crypt, LVM root/swap/home
groups <login>                     # includes sudo and user42
sudo ufw status                    # only 4242 (v4 and v6)
sudo aa-status                     # AppArmor loaded
sudo ss -tunlp | grep 4242         # sshd listening
sudo chage -l <login>              # max 30, min 2, warn 7
sudo chage -l root                 # same values
ls /var/log/sudo/                  # sudo logs exist
```

Also test these behaviors, because the evaluator will:

- `ssh root@127.0.0.1 -p 4242` is **refused**.
- A wrong `sudo` password shows **your custom message** and allows only 3 attempts.
- `passwd` with a weak password (for example `abc`) is **rejected**.
- The monitoring broadcast appears on its own every 10 minutes, with no errors.


# Part 6 - Signature and submission

1. Finish and double-check everything first.
2. Shut the VM down properly from inside it: `sudo poweroff`. Wait until VirtualBox shows **Powered Off**. Do not use "Save the machine state".
3. On the host, in the VM folder, run `sha1sum <vm-name>.vdi`.
4. Put **only the hash** in `signature.txt` at the root of your Git repository, then commit and push.
5. **Do not start that VM again.** Every boot changes the `.vdi`, which changes the hash.

- For defense practice, use **Machine &gt; Clone** in VirtualBox and practice on the clone. Avoid snapshots on the VM you submit.
- If you boot the original and change it, recompute the SHA-1 and update `signature.txt` before the defense.
- If you wait a long time before submitting, expect passwords to have **expired** (30-day policy). Changing one modifies the disk, so recompute the hash after any boot.
- Keep a backup of the VM folder on an external drive.

---
[< Previous](./04-monitoring-and-cron.md) | [Back to README](../README.md) | [Next >](./06-troubleshooting.md)
