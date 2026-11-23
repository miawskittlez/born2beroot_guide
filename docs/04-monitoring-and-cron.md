# Part 4 - The monitoring script (you write it)

> **Important:** This script is part of your grade, so it is not provided. Write it yourself and be ready to explain every line.

Create a **bash** script `monitoring.sh` that shows this on **all terminals** with `wall`, with **no errors**:

- OS architecture and kernel version
- Number of physical processors
- Number of virtual processors
- Available RAM and its usage rate (%)
- Available storage and its usage rate (%)
- CPU usage rate (%)
- Date and time of the last reboot
- Whether LVM is active
- Number of active connections
- Number of users on the server
- IPv4 address and MAC address
- Number of commands run with `sudo`

Useful reading: `man wall`, `man cron`, `man proc`. Test it by hand with `sudo`: no error lines and no empty fields.


## 4.1 Run it every 10 minutes

```bash
sudo crontab -u root -e
```

Add this line:

```bash
*/10 * * * * /usr/local/bin/monitoring.sh
```

```bash
sudo crontab -u root -l
sudo systemctl status cron
```

- It should fire at :00, :10, :20, :30, :40 and :50.
- Cron uses a minimal PATH, so test the script as cron runs it too.
- To stop it at the defense **without editing the script**: put `#` at the start of the crontab line, or run `sudo systemctl stop cron`.


## 4.2 Optional: timezone

```bash
timedatectl
sudo timedatectl set-timezone Europe/Madrid
```

---
[< Previous](./03-configure-the-server.md) | [Back to README](../README.md) | [Next >](./05-final-checks-and-signature.md)
