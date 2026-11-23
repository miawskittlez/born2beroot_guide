# Troubleshooting

| Problem | Fix |
|---|---|
| Disk erase is very slow | Cancel it (Tab to &lt;Cancel&gt;). Nothing is lost on a new disk. |
| No Primary/Logical question | GPT table: use the layout in the note under 2.2. |
| Can't paste into the VM | SSH from your host and paste there, or type it. |
| SSH: connection closed / `kex_exchange_identification` | Fill in Host IP `127.0.0.1` and Guest IP `10.0.2.15`, check NAT, `ufw status` and `ss -tunlp`, or try another host port (4243 &gt; 4242). |
| SSH still on port 22 | `sudo systemctl disable --now ssh.socket && sudo systemctl enable --now ssh` |
| sudo ignores your config | File name without `.` or `~`, permissions `0440`, then `sudo visudo -c`. |
| Wrong hostname | See 3.2. |
| Broadcast at the wrong minutes | The crontab line must be `*/10 * * * *`. |
| VM files missing | Use persistent storage and keep a backup. |

---
[< Previous](./05-final-checks-and-signature.md) | [Back to README](../README.md) | [Next >](./07-defense-qa.md)
