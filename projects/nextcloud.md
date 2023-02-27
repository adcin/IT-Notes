# occ

- [**Nextcloud manual - occ:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html)

Nextcloud’s occ command is Nextcloud’s command-line interface. **You must run it as your HTTP user** to ensure that the correct permissions are maintained on your Nextcloud files and directories.

**Using occ in a normal installation:**  

```shell
sudo -u www-data php occ <OCC_COMMAND>
```

**Using occ in a Docker Container:**  

```shell
docker exec --user www-data <CONTAINER_ID> php occ <OCC_COMMAND>
```

## Some useful occ commands  

[**Activate/Deactivate maintenance mode:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#maintenance-commands)  

```shell
sudo -u www-data php occ maintenance:mode --on
sudo -u www-data php occ maintenance:mode --off
```

```shell
docker exec --user www-data <CONTAINER_NAME> php occ maintenance:mode --on
docker exec --user www-data <CONTAINER_NAME> php occ maintenance:mode --off
```

[**Add missing DB indices:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#add-missing-indices)  

```shell
sudo -u www-data php occ db:add-missing-indices
```

```shell
docker exec --user www-data <CONTAINER_NAME> php occ db:add-missing-indices
```

[**Cleanup filecache**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#file-operations)

```shell
sudo -u www-data php occ files:cleanup
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ files:cleanup
```  

[**Rescan filesystem**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#file-operations)

```shell
sudo -u www-data php occ files:scan --all
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ files:scan --all
```  

[**Rescan the AppData folder**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#file-operations)

```shell
sudo -u www-data php occ files:scan-app-data
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ files:scan-app-data
```  

[**Cleanup shared storage entries:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#files-sharing)  

```shell
sudo -u www-data php occ sharing:cleanup-remote-storages
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ sharing:cleanup-remote-storages
```  

[**Update the systems data-fingerprint after a backup is restored:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#maintenance-commands)  

```shell
sudo -u www-data php occ maintenance:data-fingerprint
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ maintenance:data-fingerprint
```  

[**Update database mimetypes and update filecache:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#maintenance-commands)  

```shell
sudo -u www-data php occ maintenance:mimetype:update-db
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ maintenance:mimetype:update-db
```  

[**Update mimetypelist.js:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#maintenance-commands)  

```shell
sudo -u www-data php occ maintenance:mimetype:update-js
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ maintenance:mimetype:update-js
```  

[**Repair this installation:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#maintenance-commands)  

```shell
sudo -u www-data php occ maintenance:repair
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ maintenance:repair
```  

[**Updates the .htaccess file:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#maintenance-commands)  

```shell
sudo -u www-data php occ maintenance:update:htaccess
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ maintenance:update:htaccess
```  

[**Show list of all registered users:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#user-commands)  

```shell
sudo -u www-data php occ user:list
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ user:list
```  

[**Show information about a specific user:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#user-commands)  

```shell
sudo -u www-data php occ user:info <USERNAME>
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ user:info <USERNAME>
```  

[**Disable/Enable users:**](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/occ_command.html#disable-users)  

```shell
sudo -u www-data php occ user:disable <USERNAME>
sudo -u www-data php occ user:enable <USERNAME>
```  

```shell
docker exec --user www-data <CONTAINER_NAME> php occ user:disable <USERNAME>
docker exec --user www-data <CONTAINER_NAME> php occ user:enable <USERNAME>
```  

# cron jobs

- [Nextcloud manual - cron jobs](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/background_jobs_configuration.html#cron-jobs)

On my Nextcloud Docker installation, i had trouble running the cronjobs. My workaround was a cronjob on the host, which starts the Nextclound cronjob inside the Docker Container every 5 minutes.

1. **Create the script:**  

```shell
nano /path/to/my/nextcloud_cron_script.sh
```

```shell
#!/bin/bash

nextcloud_status=$(docker ps -f name='<NEXTCLOUD_CONTAINER_NAME>' --format "{{.State}}")

if [ nextcloud_status="running" ]
 then
    docker exec -u www-data -t <NEXTCLOUD_CONTAINER_NAME> php -f /var/www/html/cron.php
fi
```

2. **Make script executable:**  
```shell
chmod +x /path/to/my/nextcloud_cron_script.sh
```

3. **Test the script:**  
```shell
/path/to/my/nextcloud_cron_script.sh
#or
./nextcloud_cron_script.sh
```

4. **Create the cronjob:**
```shell
crontab -e
```

```shell
*/5 * * * * /path/to/my/nextcloud_cron_script.sh
```

5. **Check your [Admin Page](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/security_setup_warnings.html) for any Background Job warnings.**
