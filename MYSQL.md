# MySQL-Kubernetes
- Need to create `mysql-deploymet.yml` file and `mysql-service.yml` file.
## Errors:
- Error that authentication module is not supported.
> for that we create one file name `1.sql ` and it to the one minikube container directory.
- in my case I have create directory in minikube container through ssh into it.
  ```docker exec -it <container_name> sh```
  created directory `imp/data`, create file `1.sql` and paste following query.
  ```
  GRANT ALL PRIVILEGES ON *.* TO 'directus'@'%' WITH GRANT OPTION;
  ALTER USER 'directus'@'%' IDENTIFIED WITH mysql_native_password BY '171726';
  FLUSH PRIVILEGES;
  ```
- Now need to attach it to the pods that are being create with deplyoment using following code.
  ```
  spec:
  containers:
    - name: mysql
        ...
      volumeMounts:
        - name: start-sh
          mountPath: /docker-entrypoint-initdb.d
  volumes:
        - name: start-sh
          hostPath:
            path: "/imp/data"
            type: Directory
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
# Final Deployment file with services included.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deploy
  labels:
    name: database-deploy
    app: fotofizz
spec:
  replicas: 1
  selector:
    matchLabels:
      name: database-pod
      app: fotofizz
  template:
    metadata:
      name: mysql-pod
      labels:
        name: database-pod
        app: fotofizz
    spec:
      containers:
        - name: db
          image: mysql:8.1.0
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_USER
              value: "directus"
            - name: MYSQL_DATABASE
              value: "direct"
            - name: MYSQL_PASSWORD
              value: "171726"
            - name: MYSQL_ROOT_PASSWORD
              value: "root"
          # command: ['mysqld']
          # args: ['--character-set-server=utf8mb4', '--collation-server=utf8mb4_bin', '--default-authentication-plugin=mysql_native_password']
          volumeMounts:
            - name: start-sh
              mountPath: /docker-entrypoint-initdb.d
      volumes:
        - name: start-sh
          hostPath:
            path: "/imp/data"
            type: Directory

---
apiVersion: v1
kind: Service
metadata:
  name: mmysql-service
  labels:
    name: database-service
    app: fotofizz
spec:
  ports:
  - port: 3306
    targetPort: 3306
  selector:
    name: database-pod
    app: fotofizz
```
