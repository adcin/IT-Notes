# SQL

## Login

Login as root (-uroot) in the mysql shell with password query (-p):
```sql
mysql -uroot -p
```

Login as root and insert password in the command (No space after -p!):
```sql
mysql -uroot -p<root_passwort>
```

## Show info

Show all database names:
```sql
SHOW DATABASES;
```

The users are managed by the mysql.user database. List all users:  
```sql
SELECT User, Host, FROM mysql.user;
```

List all available fields in mysql.user:  
```sql
desc mysql.user;
```

Show current logged in users:  
```sql
SELECT user, host,db, command FROM information_schema.processlist;
```

Check the privileges for a specific user:  
```sql
SHOW GRANTS FOR <USERNAME>;
or
SHOW GRANTS FOR <USERNAME>@<HOST>;
```

## Backup table

Create new table as backup of an existing table:  
```sql
CREATE TABLE new_tbl AS SELECT * FROM orig_tbl;
```

Restore backup table to original table:
```sql
INSERT INTO target_tbl SELECT * FROM orig_tbl;
```

## Create table

Create empty table with the structure of an orig_tbl: 
```sql
CREATE TABLE new_tbl LIKE orig_tbl;
```

## Deleting

Delete table: 
```sql
DROP TABLE tbl;
```

Delete table and avoid errors: 
```sql
DROP TABLE IF EXISTS tbl;
```

Delete database:
```sql
DROP DATABASE <DB_NAME>;
```

Delete user:  
```sql
DROP USER <USERNAME>@<HOST>;
```

## Backup and restore

Backup database into file:  
```sql
mysqldump -u <user> -p<password> <db_name> > <backup-file>.sql
```

Restore database from file:  
```sql
mysql -u <user> -p<password> <db_name> < <backup-file>.sql
```



## Manage Databases in a Docker container

Execute SQL commands in Docker container: 
```shell
docker exec -it <Containername/-ID> mysql -uroot -p<root-passwort> -e "<mysql_command>"
```

Create a database in a running mariadb Docker container and set permissions to an existing user (with password query):  
```shell
docker exec -it <Containername/-ID> mysql -uroot -p -e "CREATE DATABASE IF NOT EXISTS <new_db> COLLATE = 'utf8mb4_general_ci'; GRANT ALL PRIVILEGES ON <new_db>.* TO '<user-name>'@'%'; FLUSH PRIVILEGES;"
```

Same, but without password query: 
```shell
docker exec -it <Containername/-ID> mysql -uroot -p<root-passwort> -e "CREATE DATABASE IF NOT EXISTS <new_db> COLLATE = 'utf8mb4_general_ci'; GRANT ALL PRIVILEGES ON <new_db>.* TO '<user-name>'@'%'; FLUSH PRIVILEGES;"
```

|      Option      | Description     |
|:----------------:| --------------- |
|      -uroot      | --user="root"   |
|        -p        | Password query  |
| -e "\<Command\>" | Execute command | 


Create new user and database in a mariadb Docker container and set permissions, all in one stroke: 
```shell
docker exec -it <container_name/-ID> mysql -uroot --password="<root-passwort>" -e "CREATE USER '<new_user>'@'%' IDENTIFIED BY '<new_password>'; CREATE DATABASE IF NOT EXISTS <new_db> COLLATE = 'utf8mb4_general_ci'; GRANT ALL PRIVILEGES ON <new_db>.* TO '<user-name>'@'%'; FLUSH PRIVILEGES;"
```

--------------

# Links

- [MariaDB Foundation](https://mariadb.org/)
- [Official MariaDB Docker Image](https://hub.docker.com/_/mariadb)
- [SQL syntax checker](https://www.eversql.com/sql-syntax-check-validator/)
