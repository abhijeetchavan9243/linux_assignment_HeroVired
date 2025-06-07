#### Create new local git repository

``` 
git init
touch readme.md
git remote add origin https://github.com/abhijeetchavan9243/linux_assignment_HeroVired.git
git branch -m master main
git add readme.md
git commit -m "first commit"
git push -u origin main
```

# Task 1 System Monitoring Setup

#### Install monitoring setup
##### htop: real-time view of CPU, memory, and process usage.
##### nmon: interactive system performance tool.
``` 
sudo apt update
sudo apt install -y htop nmon crontab
```

#### Create a monitoring script file
`sudo nano /usr/local/bin/system_monitor.sh`

##### Paste the below script in the file
``` 
#!/bin/bash
LOG_DIR="/var/log/monitor"
DATE=$(date +%F)

mkdir -p $LOG_DIR

# htop snapshot
# htop -b -n 1 > $LOG_DIR/htop_$DATE.log

# top snapshot
top -b -n 1 > $LOG_DIR/top_$DATE.log

# disk usage
df -h > $LOG_DIR/disk_usage_$DATE.log
du -sh /home/* >> $LOG_DIR/disk_usage_$DATE.log

# top processes
ps aux --sort=-%cpu | head -10 > $LOG_DIR/high_usage_$DATE.log
ps aux --sort=-%mem | head -10 >> $LOG_DIR/high_usage_$DATE.log
```

![top -b -n 1](/Users/abhijeetchavan/Developer/hero-vired/linux_assignment_HeroVired/images/Screenshot 2025-06-07 at 3.46.54 PM.png)
![df -h](/Users/abhijeetchavan/Developer/hero-vired/linux_assignment_HeroVired/images/Screenshot 2025-06-07 at 3.47.33 PM.png)
![du -sh](/Users/abhijeetchavan/Developer/hero-vired/linux_assignment_HeroVired/images/Screenshot 2025-06-07 at 3.48.03 PM.png)
![ps aux --sort=-%cpu](/Users/abhijeetchavan/Developer/hero-vired/linux_assignment_HeroVired/images/Screenshot 2025-06-07 at 3.48.30 PM.png)
![ps aux --sort=-%mem](/Users/abhijeetchavan/Developer/hero-vired/linux_assignment_HeroVired/images/Screenshot 2025-06-07 at 3.48.45 PM.png)

#### To make script executable
`chmod +x /usr/local/bin/system_monitor.sh`

#### Edit cron tab and add new cron job for running script automatically
``` 
crontab -e
0 1 * * * /usr/local/bin/system_monitor.sh
sudo systemctl status cron
```

## htop does not run non-interactively hence using top
---