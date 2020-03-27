# How to do

### 1. clone the repository to your docker server

```shell
git clone https://github.com/codlin/www
```

### 2. copy a file from .env_template, named it .env

### 3. fill in mysql info in the .env file

### 4. applay ssl certification

Refer to:

```
https://medium.com/@pentacent/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71
```

Please follow these steps:

1. Edit the script to add in your domain(s) and your email address. If you’ve changed the
   directories of the shared Docker volumes, make sure you also adjust the data_path variable as well.

2. run sh script

   ```shell
   chmod +x init-letsencrypt.sh
   sudo ./init-letsencrypt.sh
   ```
   or
   ```shell
   docker run --rm -p 80:80 -p 443:443  -v /data/www/certbot:/etc/letsencrypt quay.io/letsencrypt/letsencrypt auth --standalone -m 4106807@qq.com --agree-tos -d poding.net -d www.poding.net
   ```
   
### 5. clone https://gitlab.com/poding/appbass.git

Clone repository into path /data/www/appbass, which is defined in docker-compose.yaml (appbass: build)

### 6. create mysql database

Create database and user info according to .env

```shell
mysql -u root -p
CREATE USER '${your.user.in.env}'@'%' IDENTIFIED BY '${your.password.in.en}';
CREATE DATABASE ${your.database.in.env};
use ${your.database.in.env};
grant usage on *.* to '${your.user.in.env}'@'%' IDENTIFIED BY '${your.password.in.env}';
GRANT all privileges on ${your.database.in.env}.* to '${your.user.in.env}'@'%';
FLUSH PRIVILEGES;
```

REPLACE ${your.user.in.env}, ${your.password.in.en} and \${your.database.in.env} with real names, e.g.:

```shell
CREATE USER 'admin'@'%' IDENTIFIED BY 'RTGVTGJHJ';
```

### 7. build and start docker service

```shell
docker-compose up -d
```

Please enjoy yourself!
