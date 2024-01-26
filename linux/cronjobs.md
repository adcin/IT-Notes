# Crontab

Crontab maintains crontab files for individual users.

usage: `crontab [ -u user ] [ -i ] { -e | -l | -r }`  

| Option | Description                           |
| ------ | ------------------------------------- |
|        | default operation is replace          |
| -e     | edit user's crontab                   |
| -l     | list user's crontab                   |
| -r     | delete user's crontab                 |
| -i     | prompt before deleting user's crontab |

Example: list current user's crontab

```shell
crontab -l
```


## Create a cronjob in crontab
  
Create a cronjob for the current user:
  
```shell
crontab -e
```

If you run the command for the first time, you might be asked which texteditor you want to use.  

Structure of the crontab file:

```text
*  *  *  *  *  Command to be executed
-  -  -  -  -
|  |  |  |  |
|  |  |  |  +-- Weekday (0 - 7) (Sunday is 0 and 7; alternatively mon, tue, wed, ...)
|  |  |  +---- Month (1 - 12, alternatively jan, feb, mar, ...)
|  |  +------ Day (1 - 31)
|  +-------- Hour (0 - 23)
+---------- Minute (0 - 59)
```

| Notation | Description                                    |
| -------- | ---------------------------------------------- |
| \*       | any minute, hour, day, ...                     |
| \*\/2    | every 2 minutes, hours, days, ...              |
| 1,7,12   | at 1, 7, 12 minute, hour, day, ...             |
| 1-12     | until 12 every 1 minute, hour, day, ...        |
| 1-12\/2  | from 1 to 12 every 2 minutes, hours, days, ... | 

The output of the command is sent to the mail of the user. You ca redirect it with:
` > /dev/null` send just the error messages to the mail.
` > /dev/null 2>&1` redirect all messages to `/dev/null`.

### Examples:  

Execute myscript.sh daily at 00:05:  
`5 0 * * * $HOME/directory/myscript.sh >> $HOME/tmp/out 2>&1`

Execute docker nextcloud command every 5 minutes:  
`*/5 * * * * docker exec -u www-data -t nextcloud php -f /var/www/html/cron.php`

Execute ping from monday to friday at 00:22:  
`0 22 * * 1-5 ping -c 1 www.google.de`

Execute backup.sh every 3 days:  
`0 3 */3 * * /private-backup/backup.sh > /dev/null 2>&1`  

### Attention!  
- When the system is not running at the defined time, the task will NOT be executed!  
- `* */2 * * * srcipt.sh` will execute the script.sh every 2 hours, but then 60 times (every minute).  

# Links & Resources
- [baeldung.com - Temporarily or Permanently Disable Every Job Listed in a Crontab](https://www.baeldung.com/linux/crontab-disable-jobs)
- [baeldung.com - How to Backup Cron Files](https://www.baeldung.com/linux/cron-files-backup#location-of-anacron-and-cron-configuration-files)
