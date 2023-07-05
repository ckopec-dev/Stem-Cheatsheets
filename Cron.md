
# Cron Cheatsheet

## Administration

### Show existing cron jobs

`$ crontab -l`

### Edit crontab

`$ crontab -e`

### Crontab entries

~~~

* * * * * command to execute

1st star: minute (0-59)
2nd star: hour (0-23)
3rd star: day of month (1-31)
4th star: month (1-12)
5th star: day of week (0-6; Su-Sa)

Default is *, which means run every interval.
To run multiple times per interval, separate values with a commad. E.g. 15,30,45 * 1 * *

~~~
