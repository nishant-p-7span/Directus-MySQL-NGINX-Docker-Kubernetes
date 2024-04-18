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
