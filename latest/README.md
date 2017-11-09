This is a simple Apache image, including PHP and SSL support. In order to use this image effectively, you'll need to mount:

    /var/www/html for your site content (e.g. using “-v /home/piuma/mysite/:/var/www/html”)

ErrorLogs are printed to output.

## Simple Examples

   ```docker run -p 80:80 -p 443:443 piuma/centos7-apache-php```

and browse to the host's IP address using http or https to show phpinfo page.


Assuming you have your content at /home/piuma/mysite/, the below will be sufficient to serve it.

   ```docker run -p 80:80 -p 443:443 -d -v /home/piuma/mysite/:/var/www/html piuma/centos7-apache-php```

## Send Email by msmtp

To send email you can write a configuration for msmtp in /etc/msmtprc.
PHP is already configured to use msmtp to send emails.

   ```docker run -d -v /home/piuma/config/msmtprc:/etc/msmtprc piuma/centos7-apache-php```

Simple msmtp configuration example:
```
defaults

port 587
tls on

account myaccount

host smtp.example.com
domain mydomain.com
from info@mydomain.com
auto_from off
maildomain mydomain.com
# Get the fingerprint with
# $ msmtp --serverinfo --tls --tls-certcheck=off --host=smtp.freemail.example
tls_fingerprint D0:83:13:3D:99:99:1F:81:1A:B3:58:C4:51:27:13:81:45:8E:74:F4
auth on
user info@mydomain.com
password mypassword

account default : myaccount
```

## Docker Compose

An example to use the container in docker-compose in combination with MySql database
```
version: "2"
services:

  web:
    image: piuma/centos7-apache-php
    container_name: web
    volumes:
      - /home/piuma/mysite/:/var/www/html
      - /home/piuma/config/msmtprc:/etc/msmtprc
    environment:
      LANG: it_IT.UTF-8
    ports:
      - "80:80"
      - "443:443"
    links:
      - db

  db:
    image: mariadb:5
    container_name: db
    environment:
      - MYSQL_ROOT_PASSWORD=mysecret
      - MYSQL_DATABASE=mydb
      - MYSQL_USER=myuser
      - MYSQL_PASSWORD=mypass
      - TERM=dumb
    volumes:
      - /home/piuma/mydb/data:/var/lib/mysql
      - /home/piuma/mydb/conf:/etc/mysql/conf.d
      - /initdb.d:/docker-entrypoint-initdb.d
```
