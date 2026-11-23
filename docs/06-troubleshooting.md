# Troubleshooting

| Problem | Likely cause and fix |
|---|---|
| Hostname shows `debian<login>42` | The installer pre-filled `debian`. Fix with `hostnamectl set-hostname` and edit `/etc/hosts` (section 3.2). |
| Disk erase during encryption is very slow | Normal on a virtual disk. You can cancel it (Tab to &lt;Cancel&gt;); nothing is lost on a new disk. |
| Partition setup never asks Primary/Logical | GPT table. Use the 1 MB BIOS boot partition + /boot + encrypted partition layout (note in 2.2). |
| Cannot paste into the VM | No clipboard sharing on a minimal server. Use SSH from the host and paste there, or type carefully. |
| `visudo`: syntax error | A `Defaults` line was split across two lines. Press `e`, join it, save. Never press `Q`. |
| `visudo -c`: bad permissions, should be mode 0440 | `sudo chmod 0440 /etc/sudoers.d/sudo_config` (four digits, not 04440). |
| SSH: `kex_exchange_identification` / connection closed | Fill in Host IP `127.0.0.1` and Guest IP `10.0.2.15` in the port-forwarding rule; check NAT mode, `ufw status`, `ss -tunlp`; try another host port (for example 4243 &gt; 4242). |
| Still listening on port 22 | `sudo systemctl disable --now ssh.socket && sudo systemctl enable --now ssh`. |
| Script prints blank fields or awk/tail errors | Typo in a variable name or expression (see section 4.1). |
| Broadcast arrives at the wrong minutes | Check the crontab line: it must be `*/10 * * * *`. |
| `sudo` ignores your config | Check the file name (no `.` or `~`), permissions 0440 and run `sudo visudo -c`. |
| VM files disappeared | Storage was wiped or full. Use persistent storage and keep a backup. |

---
[< Previous](./05-final-checks-and-signature.md) | [Back to README](../README.md) | [Next >](./07-defense-qa.md)
