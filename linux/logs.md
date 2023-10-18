# Locations  

| Path        | Description    |
|:----------- |:-------------- |
| `/var/log/` | main directory |


# Tools  

## `less`  

## `dmesg`  

`dmesg | less`  

## `tail`  

`tail -f -n 5 /var/log/syslog`  

## `journalctl`

```shell
journalctl --since="2023-10-18 01:00:00" --until="2023-10-18 01:30:00"
_PID=4242
--grep="PATTERN"
journalctl -u apache.service -u php-cgi.service -u mysqld.service
--output=short-full
```

# Work in progress notes

https://www.linuxfoundation.org/blog/blog/classic-sysadmin-viewing-linux-logs-from-the-command-line
https://www.cyberciti.biz/faq/linux-log-files-location-and-how-do-i-view-logs-files/
