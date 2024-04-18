# directus on Docker

* Command to run docker container with directus.
>First need to --link mysqlserver to our directus container.
```
docker run -it -p 8055:8055 --name app -e DB_CLIENT=mysql -e DB_HOST=sql_server -e DB_PORT=3306 -e DB_DATABASE=direct -e DB_USER=nishant -e DB_PASSWORD=root -e KEY=hello -e SECRET=hiii --link sql_server:mysql directus/directus
```
