---
cover: https://academy.hackthebox.com/storage/modules/18/logo.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Linux Fundamentals

## Task Scheduling

### Systemd Tool

1. Create a time
2. Create a service
3. Activate the time

```
sudo mkdir /etc/systemd/system/mytime.time.d  # the directory where the timer is stored in
sudo vim /etc/systemd/system/mytimer.timer    # the timer
```

| Label           | Description                                    |
| --------------- | ---------------------------------------------- |
| \[Unit]         | ...                                            |
| \[Timer]        | ...                                            |
| \[Install]      | ...                                            |
| Description     | The description for the timer.                 |
| OnBootSec       | Execute the script once after the system boot. |
| OnUnitActiveSec | Run the script at regular intervals.           |

```
# /etc/systemd/system/mytimer.timer
[Unit]
Description=My Timer
​
[Timer]
OnBootSec=3min
OnUnitActiveSec=1hour
​
[Install]
WantedBy=timers.target
```

Create the service.

```
sudo vim /etc/systemd/system/mytimer.service
```

```
# /etc/systemd/system/mytimer.service
[Unit]
Description=My Service
​
[Service]
ExecStart=/full/path/to/my/script.sh
​
[Install]
WantedBy=multi-user.target
```

Reload, start and enable the systemd.

```
sudo systemctl daemon-reload
sudo systemctl start mytimer.service
sudo systemctl enable mytimer.service
```

### Crontab Tool

| **Time Frame**         | **Description**                                                       |
| ---------------------- | --------------------------------------------------------------------- |
| Minutes (0-59)         | This specifies in which minute the task should be executed.           |
| Hours (0-23)           | This specifies in which hour the task should be executed.             |
| Days of month (1-31)   | This specifies on which day of the month the task should be executed. |
| Months (1-12)          | This specifies in which month the task should be executed.            |
| Days of the week (0-7) | This specifies on which day of the week the task should be executed.  |

The crontab file.

```
# System Update
* */6 * * /path/to/update_software.sh
​
# Execute scripts
0 0 1 * * /path/to/scripts/run_scripts.sh
​
# Cleanup DB
0 0 * * 0 /path/to/scripts/clean_database.sh
​
# Backups
0 0 * * 7 /path/to/scripts/backup.sh
```

## Questions

1. What is the name of the config file that has been created after 2020-03-03 and is smaller than 28k but larger than 25k?

```
find / -newermt 2020-03-03 -name *.conf -type f -size +25k -size -28k -exec ls -hls {} \; 2>/dev/null
```

2. How many files exist on the system that have the ".bak" extension?

```
find / -type f -name *.bak -exec echo {} \; 2>/dev/null
```

3. Submit the full path of the "xxd" binary.

```
find / -type f -name *xxd 2>/dev/null
```

4. How many files exist on the system that have the ".log" file extension?

```
find / -type f -name *.log 2>/dev/null | wc -l
```

5. How many total packages are installed on the target system?

```
dpkg -l | grep -c '^ii'
```

6. How many services are listening on the target system on all interfaces? (Not on localhost and IPv4 only)

```
netstat -lnp4 | grep -e 'LISTEN' | grep -vc '127.0.0'
```

7. Determine what user the ProFTPd server is running under. Submit the username as the answer.

```
ps -aux | grep proftpd | cut -d " " -f1
```

8. Use cURL from your Pwnbox (not the target machine) to obtain the source code of the "[https://www.inlanefreight.com](https://www.inlanefreight.com/)" website and filter all unique paths of that domain. Submit the number of these paths as the answer.

## System Management

1. What is the type of the service of the "syslog.service"?

```
sudo systemctl show syslog.server -p Type
```

\
