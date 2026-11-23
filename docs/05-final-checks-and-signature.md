# Part 5 - Final checks

```bash
hostnamectl                     # <login>42
lsblk                           # /boot, sda5_crypt, LVM root/swap/home
groups <login>                  # sudo and user42
sudo ufw status                 # only 4242
sudo aa-status                  # AppArmor loaded
sudo ss -tunlp | grep 4242      # sshd listening
sudo chage -l <login>           # 30 / 2 / 7
sudo chage -l root              # 30 / 2 / 7
ls /var/log/sudo/               # logs exist
```

Also test:

- `ssh root@127.0.0.1 -p 4242` is refused.
- A wrong `sudo` password shows your message and stops after 3 tries.
- `passwd` rejects a weak password.
- The broadcast appears every 10 minutes with no errors.


# Part 6 - Signature and submission

1. Finish every check first.
2. Shut down from inside the VM: `sudo poweroff`. Wait for **Powered Off**.
3. On the host, in the VM folder: `sha1sum <vm-name>.vdi`.
4. Put **only the hash** in `signature.txt` at the root of your Git repo. Commit and push.
5. **Do not start that VM again**: every boot changes the hash.

- Practice on a **clone** (Machine &gt; Clone), not on the submitted VM.
- If you boot the original, recompute the hash and update `signature.txt`.
- Passwords expire after 30 days, so an old VM asks for a new password at login (this changes the disk).

---
[< Previous](./04-monitoring-and-cron.md) | [Back to README](../README.md) | [Next >](./06-troubleshooting.md)
