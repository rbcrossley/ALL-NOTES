Various configuration examples provided here

# Config-01
```
/var/log/apache2/*.log {
        weekly
        missingok
        rotate 52
        compress
        delaycompress
        notifempty
        create 640 root adm
        sharedscripts
        postrotate
            /etc/init.d/apache2 reload > /dev/null
        endscript
        prerotate
            if [ -d /etc/logrotate.d/httpd-prerotate ]; then \
                    run-parts /etc/logrotate.d/httpd-prerotate; \
            fi; \
        endscript
}
```

- weekly: rotate logs once per week. Available options are daily, weekly, monthly, and yearly.
- missingok: If no \*.log files are found, don't freak out. It'll be handled properly.
- rotate 52: Keep 52 files before deleting old log files. (That's a default of 52 weeks, or one years worth of logs!).
- compress: Compress (gzip) log files
	- delaycompress: Delays compression until 2nd time around rotating. As a result, you'll have one current log, one older log which remains uncompressed, and then a series of compressed logs.
	- compresscmd: Set which command to use to compress. Defaults to gzip.
	- uncompresscmd: Set the command to use to uncompress. Default to gunzip.
- notifempty: Don't rotate empty files.
- create 640 root adm: Create new log files with set permissions, owner and group. This example creates a file with user root and group adm. 
- sharedscripts: Run postrotate script after all logs are rotated. If this is not set, it will run postrotate after each matching file is rotated.
- postrotate: Scripts to run after rotating is done. In this case, Apache is reloaded so it writes to the newly created log files. 
- prerotate: Run before log rotation begins.


# Config-02
```
/home/techyman/glassfish4/glassfish/domains/*/logs/*
{
su techyman techyman
copytruncate
rotate 5
daily
compress
dateext
missingok
notifempty
}
```

# Config-03
```
/home/techyman/glassfish4/glassfish/domains/*/logs/*.log {
        daily
        missingok
        rotate 30
        compress
        delaycompress
        notifempty
}
```
When logrotate was forced with  `logrotate -f glassfish `, what happened is that at first `application.log` and `application.log.1` was created, but after again forcing logrotate, it was zipped as `application.log.2.gz` with only `application.log.1` remaining there.

# References:
https://serversforhackers.com/c/managing-logs-with-logrotate