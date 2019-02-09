# MySql on GCP Notes

### Mysql Shell Connection
* Keep open your cloud_sql_proxy
* Open CMD
* Change current directory to C:\Program Files\MySQL\MySQL Shell 1.0\bin
* Once there, execute: > mysqlsh -u root -p -P 3307 --host 127.0.0.1


### Set up Event Scheduler
* The event scheduler option can now be configured via flags
* Go to Google DB Console and set the following flag: event_scheduler on
* Query the correct activation
```sql
SHOW PROCESSLIST;

SELECT @@GLOBAL.event_scheduler;

SHOW events;
```