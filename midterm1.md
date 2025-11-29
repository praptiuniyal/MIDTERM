## Experiment: [Daily System Logger Script]
### Name: Prapti Uniyal, Roll No.: 590028360 Date: 2025-10-19

### AIM:
* To create a shell script that logs current system information, rotates old logs, and schedules itself to run daily.

### Requirements:
* Any Linux Distro  
* Any text editor (VS Code, Vim,etc.)  
* Cron service for scheduling  


### Theory:

In this task, I created a script that helps keep track of what’s happening on the computer every day — like who’s using it, how much space is left, and what programs are running.

Here’s what the script does:
- It saves important system info like the username, date, running processes, and disk usage.

- It keeps things clean by automatically saving old logs.

- It runs by itself every day at a fixed time using something called cron, so I don’t have to do it manually.

This makes it easier to monitor the system and catch any problems early.


### Procedure & Observations

#### **Exercise 1: Creating the Daily Log Script**

##### **Task Statement:**
Write a shell script that logs system info and handles automatic rotation of old logs.

##### **Explanation:**
 The script-

- checks who is using the system.
- makes a folder to keep log files safe.
- saves a new log every day with the date and time.
- cleans up old logs that are more than 7 days old.
- can be set to run automatically every day using cron.


##### **Command(s):**

```
#!/bin/bash
USER_NAME=$(whoami)

LOG_DIR="/home/$USER_NAME/daily_logs"
mkdir -p "$LOG_DIR"


DATE=$(date +"%Y-%m-%d_%H-%M-%S")
LOG_FILE="$LOG_DIR/system_log_$DATE.txt"


{
    echo "============================================"
    echo "  Daily System Log for $USER_NAME"
    echo "  Date: $(date)"
    echo "============================================"
    echo ""
    echo "---- System Uptime ----"
    uptime
    echo ""
    echo "---- Disk Usage ----"
    df -h
    echo ""
    echo "---- Memory Usage ----"
    free -h
    echo ""
    echo "---- Logged-in Users ----"
    who
    echo ""
} > "$LOG_FILE"

find "$LOG_DIR" -type f -name "*.txt" -mtime +7 -exec rm {} \;
"midterm_log.sh" 36L, 755B         
```
Output :

<p align="center">
<img src="/img/m3.png" width="900">
</p>

<p align="center">
<img src="/img/m6.png" width="900">
</p>



**Result:**


A shell script was successfully created to collect daily system information such as uptime, disk usage, memory status, and the current user. The log is saved with a timestamp in a dedicated folder, making it easy to track system activity over time. The script runs smoothly and prepares the log file for further actions like emailing or archiving.




#### Exercise 2: Scheduling the Script

**Task Statement**:
Schedule the above script to run daily using cron.

Explanation:
Use crontab to automate the script execution at a fixed time every day.

Command(s):
```
bash

crontab -e
20 8 * * * /mnt/c/Users/ASUS/Desktop/day7/final_log.sh
```

Output :

<p align="center">
<img src="/img/m1.png" width="900">
</p>



**Result:**
The script successfully logs daily system information, archives logs older than 7 days, and schedules itself to run daily using a cron job.


