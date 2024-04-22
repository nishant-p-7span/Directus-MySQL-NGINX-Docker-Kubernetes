# How to Set up NGINX with docker continer.

## First let's look at the compose file:
```
ngin:
    container_name: nginx
    depends_on:
      - app
    image: nginx
    restart: unless-stopped
    ports:
      - 80:80
    volumes:
      - ./nginx:/etc/nginx/conf.d
    links:
      - app:directus/directus
```

* we are exposing 80:80 port to bypass port using on link.
* here we mount local storage which contain nginx config file to container's conf.d file.
## Conf.d file
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://app:8055;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }

        location ~ /.well-known/acme-challenge {
                allow all;
                root /var/www/html;
        }
}
```
* **proxy_pass http://app:8055;** here app is name of container in services. (for more explaination view compose.yaml file).
