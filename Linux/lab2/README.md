# Lab 2: Shell Scripting Basics

## Objective
Create a shell script that automates checking disk space usage and sends an email alert if usage exceeds a specified threshold.

## Prerequisites
- Basic knowledge of shell scripting.
- Access to a Unix-like operating system (Linux or macOS).
- `mail` utility installed for sending email alerts.

## Steps

### 1. Create the Shell Script
1. Open your terminal.
2. Create a new shell script file:
   ```
   touch check_disk_space.sh
3. Make the script executable
   ```
   chmod +x check_disk_space.sh
4. Write the Script
Open the check_disk_space.sh file with a text editor and add the following content:
```
#!/bin/bash

# Threshold for disk usage (e.g., 80 for 80%)
THRESHOLD=80

# Email to send the alert to
EMAIL="your_email@example.com"

# Get the current disk usage percentage for the root filesystem
USAGE=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

# Check if the usage exceeds the threshold
if [ "$USAGE" -gt "$THRESHOLD" ]; then
  # Send an email alert
  SUBJECT="Disk Space Alert: Usage is at ${USAGE}%"
  MESSAGE="Warning: Disk space usage has exceeded the threshold of ${THRESHOLD}%. Current usage is ${USAGE}%."
  echo "$MESSAGE" | mail -s "$SUBJECT" "$EMAIL"
fi

4. Automate the Script with Cron
To automate the script and run it at regular intervals, add a cron job:
    Open the cron table for editing:
        ```
        crontab -e
    Add a new line to run the script every hour:
         ```
            0 * * * * /path/to/your/check_disk_space.sh
