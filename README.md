# Raspberry OS doesn't enable syslog messaging, to install
sudo apt update && sudo apt install rsyslog

# Limit size of syslog files

sudo vi /etc/logrotate.d/rsyslog
```
/var/log/syslog
{
    rotate 1
    hourly
    maxsize 1G # add this line
    missingok
    notifempty
    delaycompress
    compress
    postrotate
        /usr/lib/rsyslog/rsyslog-rotate
    endscript
}
```

sudo systemctl restart rsyslog.service

This will force your syslog to "rotate" (i.e., create a new log file and archive the previous log file) after either 1 day or when the file becomes 1GB, whichever comes first. Note that rotate 2 means your system will only keep 2 total syslog backups so it can only ever take up 2GB of space.

NOTE:: if the above config doesn't work try copying the cron jobs

``` ucp@pi4:/etc/cron.daily $ sudo cp logrotate ../cron.hourly/
```
