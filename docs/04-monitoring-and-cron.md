# Part 4 - The monitoring script (you write it)

> **Important:** The script is a graded, explained-at-the-defense part of the project, so it is intentionally **not** included. Write it yourself.

Create a bash script named `monitoring.sh` (for example in `/usr/local/bin/`) that displays the following information to **all terminals** using `wall`, with **no errors**:

- Architecture of the OS and its kernel version
- Number of physical processors
- Number of virtual processors
- Available RAM and its utilization rate as a percentage
- Available storage and its utilization rate as a percentage
- Current CPU utilization rate as a percentage
- Date and time of the last reboot
- Whether LVM is active or not
- Number of active connections
- Number of users using the server
- IPv4 address and MAC address
- Number of commands executed with `sudo`

The banner is optional. Read the manual pages (`man wall`, `man proc`, `man cron`) and the output of the standard monitoring tools on your own system to decide how to collect each value.


## 4.1 Test it

- Run it by hand with `sudo` and check that **no error lines** appear and **no field is blank** (blank fields usually mean a typo in a variable name or a wrong `awk` expression).
- Run a few `sudo` commands first so the sudo counter is not zero.
- Common typos: `$(` written as `$`, `tail -1` written as `tail 1`, `$15` written as `15$`, `/` typed as `?`.


## 4.2 Run it every 10 minutes with cron

```bash
sudo crontab -u root -e
```

Add this line (the `/` after the first `*` and the exact file name matter):

```bash
*/10 * * * * /usr/local/bin/monitoring.sh
```

```bash
sudo crontab -u root -l
sudo systemctl status cron
```

- It should fire at :00, :10, :20, :30, :40 and :50 only. If it fires at other minutes, your schedule is wrong.
- Cron runs with a **minimal PATH**, so make sure your script works when run by cron, not only by hand.
- At the defense you must **interrupt it without modifying the script**: comment out the line in the crontab (put a `#` at its start) or run `sudo systemctl stop cron`.


## 4.3 Optional: timezone

The timezone is not graded, but if the clock looks wrong: `timedatectl`, then for example `sudo timedatectl set-timezone Europe/Madrid`.

---
[< Previous](./03-configure-the-server.md) | [Back to README](../README.md) | [Next >](./05-final-checks-and-signature.md)
