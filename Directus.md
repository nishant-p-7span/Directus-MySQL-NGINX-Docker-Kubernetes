# directus on Docker

* Command to run docker container with directus.
>First need to --link mysqlserver to our directus container.
```
docker run -it -p 8055:8055 --name app -e DB_CLIENT=mysql -e DB_HOST=sql_server -e DB_PORT=3306 -e DB_DATABASE=direct -e DB_USER=nishant -e DB_PASSWORD=root -e KEY=hello -e SECRET=hiii --link sql_server:mysql directus/directus
```

* Here we need to take care that, when you create database it should be in proper collation type. to match with dump.

## when working with docker-compose, if directus atabase connectiion fail, cause mysql is not initialised then use this
```
restart: unless-stopped
```
* Add this to directus coompose file section, so it will restart again and again till we stop container.

## set mysqld default configuration like default collation and character sets:
* Add this line to docker compose file.
```
command: ['mysqld', '--character-set-server=utf8mb4', '--collation-server=utf8mb4_bin', '--mysql_native_password=ON']
```

## Directus issue when migrating database dump.
* manually install dump and run directus container on it. it will work without error.
* export dump of that database. Case all the requ. changes are done.
* now on import new dump on the start up of container and you won't have any error.

# Connecting Directus to AWS S3:
* First create One s3 bucket and enable public access.
* go to the bucket policy:
* Add the policy like this:
```
{
    "Version": "xxxx-xx-xx",
    "Id": "Policyxxxxxxxx",
    "Statement": [
        {
            "Sid": "Stmtxxxxxx",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::name_of_bucket"
        }
    ]
}
```
* create one user, with s3 full access and generate access token.
* Paste the following env variable to compose file.
```
      - STORAGE_LOCATIONS="amazon"
      - STORAGE_AMAZON_DRIVER="s3"
      - STORAGE_AMAZON_KEY="nhibataSaktaBro"
      - STORAGE_AMAZON_SECRET="YetoBilukulBhiNhi"
      - STORAGE_AMAZON_BUCKET="DeleteHoJayegi-TumDekhoUssePele"
      - STORAGE_AMAZON_REGION="ap-south-1"
      - STORAGE_AMAZON_ENDPOINT="https://s3.amazonaws.com"
      - STORAGE_AMAZON_ROOT="/uploads"
```
* it is imortant to add **https://** in `STORAGE_AMAZON_ENDPOINT`, otherwise you will encounter some error.
