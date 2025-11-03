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

![](./images/output1.1.png)

![](./images/output1.2.png)



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

![](./images/ouput2.1.png)

**Result:**
The script successfully logs daily system information, archives logs older than 7 days, and schedules itself to run daily using a cron job.


#### **Exercise 3: Enhancements and Exploration**


**Task Statement:**
Explore additional functionalities that can be added to the Daily System Logger script to improve automation, usability, and robustness.


**1. Send an Email with Log Attachment**
**2. Add error handling**
**3. Add a menu for manual archive,log cleanup or viewing latest log**


###### **Comaand(s):**
```
#!/bin/bash

LOG_DIR="$HOME/system_logs"
LOG_FILE="$LOG_DIR/daily_log_$(date +%F_%H-%M).txt"
ARCHIVE_DIR="$LOG_DIR/archive"
EMAIL="praptiuniyalofficial@gmail.com"


mkdir -p "$LOG_DIR"
mkdir -p "$ARCHIVE_DIR"


create_log() {
    echo "System Log - $(date)" > "$LOG_FILE"
    echo "User: $(whoami)" >> "$LOG_FILE"
    echo "--- Uptime ---" >> "$LOG_FILE"
    uptime >> "$LOG_FILE"
    echo "--- Disk Usage ---" >> "$LOG_FILE"
    df -h >> "$LOG_FILE"
    echo "--- Memory Usage ---" >> "$LOG_FILE"
    free -m >> "$LOG_FILE"
    echo "Log saved to $LOG_FILE"
}

send_email() {
    export MSMTP_LOGFILE=~/.msmtp.log
    echo "Hi Prapti, here is your system log." | mutt -s "System Log" -a "$LOG_FILE" -- "$EMAIL"
    echo " Email sent (check ~/.msmtp.log for status)"
}


archive_old_logs() {
    find "$LOG_DIR" -name "daily_log_*.txt" -mtime +7 -exec mv {} "$ARCHIVE_DIR" \;
    echo " Old logs moved to archive"
}

view_latest_log() {
    latest=$(ls -t "$LOG_DIR"/daily_log_*.txt 2>/dev/null | head -n 1)
    if [ -n "$latest" ]; then
        cat "$latest"
    else
        echo "  No logs found."
    fi
}


echo "Choose an option:"
echo "1) Create and email today's log"
echo "2) Archive logs older than 7 days"
echo "3) View latest log"
read -p "Enter your choice (1-3): " choice

case $choice in
 1)
        create_log
        send_email
        ;;
    2)
        archive_old_logs
        ;;
    3)
        view_latest_log
        ;;
    *)
        echo " Invalid choice"
        ;;
esac

```

![](./images/output3.1.png)

![](./images/output3.2final.png)

![](./images/output3.3.png)




**Result:**

The script successfully collects system information, saves it with a timestamp, and sends it as an email attachment. It also manages older logs by archiving them and provides a simple menu for manual tasks like viewing logs or cleaning up files. The automation works as expected when scheduled with cron, ensuring daily execution without manual effort.




