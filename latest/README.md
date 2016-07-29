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
