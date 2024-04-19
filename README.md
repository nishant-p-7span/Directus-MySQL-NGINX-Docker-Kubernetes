# MySQL-Docker

* Running MySQL On Docker.
```
docker run --name sql_server -p 3307:3306 --network=yournetworkdriver  --rm -v ${PWD}/dump:/dump -e MYSQL_ROOT_PASSWORD=root -d mysql
```
* Execute Mysql container.
```
docker exec -it sql_server /bin/bash
```
* Connecting to the mysql:
```
mysql -u root -p root
```
* Here we need to create users.
```
CREATE USER 'your_user'@'%' IDENTIFIED BY 'your_password';
```
```
GRANT ALL PRIVILEGES ON *.* TO 'your_user'@'%' WITH GRANT OPTION;
```
```
FLUSH PRIVILEGES;
```

> there might be possiblility that you still gonna have issue connecting app.
```
ALTER USER 'your_user'@'%' IDENTIFIED WITH mysql_native_password BY 'your_password';
```
```
FLUSH PRIVILEGES;
```
# Update:
* AS MYSQL8 and directus 9 have compatability issue. so switch to mysql5 for smooth database migrations. (you don;t have to do that steps)

* Found another solution that, add following line into docker compose while running it.this will solve the error.
```
command: ['mysqld', '--character-set-server=utf8mb4', '--collation-server=utf8mb4_bin', '--default-authentication-plugin=mysql_native_password']
```

## Here Few things we need to remember.
* When you are connecting to your local then our container expose to 3307 port with localhost ip
```
mysql -uroot -p -P3307 -h127.0.0.1
```
## But What if we want to connect this container to my other container?

* Let's know IP of our container
```
docker inspect mysqlserver
```
* we use --link function.
```
docker run -p 3308:3306 --name app -e MYSQL_ROOT_PASSWORD=root -d --link mysqlserver:mysql mysql
```
* now lets connect to our server
```
mysql -uroot -p -P3306 -hipofcontainer
```


# other commands:
*command to import dump of the sql
```
mysql -u username -p database_name < file.sql
```
* Restoring dump file.
```
docker exec -i some-mysql sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD"' < /some/path/on/your/host/all-databases.sql
```

# Advance Section:
* If Mysql Collation error comes:
```
ALTER DATABASE <database_name> CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;
```
```
ALTER TABLE <table_name> CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;
```
```
ALTER TABLE <table_name> MODIFY <column_name> VARCHAR(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;
```
* Create database with prefered collation type.
```
CREATE DATABASE db_name CHARACTER SET latin1 COLLATE latin1_swedish_ci;
```
