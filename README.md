
# SSL Setup on Amazon Linux 2023 with Apache web server and PHP packages 

To set up SSL on an Amazon Linux instance running Apache web server and PHP packages, you'll need to follow these general steps:

1. Connect to your instance

2. To ensure that all of your software packages are up to date, perform a quick software update on your instance. 

  ```bash
    sudo dnf update -y
  ```

3. Install the latest versions of Apache web server and PHP packages for AL2023.

  ```bash
    sudo dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
  ```

4. Install the MariaDB software packages. Use the dnf install command to install multiple software packages and all related dependencies at the same time. 

  ```bash
    sudo dnf install mariadb105-server
  ```

5. Start the Apache web server.

  ```bash
    sudo systemctl start httpd
  ```

6. Use the systemctl command to configure the Apache web server to start at each system boot.

  ```bash
    sudo systemctl enable httpd
  ```

7. You can verify that httpd is on by running the following command:

   ```bash
    sudo systemctl is-enabled httpd
  ```

8. To adjust file permissions, use the appropriate commands.

  ```bash
    sudo usermod -a -G apache ec2-user
    sudo chown -R ec2-user:apache /var/www
    sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
    find /var/www -type f -exec sudo chmod 0664 {} \;
  ```

9. Navigate to the Apache document root at /var/www/html.

  ```bash
    cd /var/www/html
  ```

10. Set the folder permission to 0775 by running this command.

  ```bash
    sudo chmod 0775 /var/www/html
  ```

11. After that, run the following command to open the Virtual Host configuration file.

```bash
    sudo vim /etc/httpd/conf.d/vhost.conf
  ```

12. Once the file is opened, please paste the following content in it. I've assumed the domain name to be engraws.xyz. Please replace it with your own domain or subdomain.

```bash
    <VirtualHost *:80>

      ServerName www.engraws.xyz
      ServerAlias www.engraws.xyz
      DocumentRoot /var/www/html
      ServerAdmin devops@engraws.xyz

      <Directory /var/www/html>
        AllowOverride All
      </Directory>
      
    </VirtualHost>
  ```

13. To install Apache and mod_ssl on Amazon Linux 2023, use the following command:

 ```bash
    sudo dnf install httpd mod_ssl
  ```

14. Restart Apache.

   ```bash
    sudo systemctl restart httpd
  ```

15. Install Certbot, a tool provided by Let’s Encrypt for easily obtaining SSL certificates, using the following command:

  ```bash
    sudo dnf install python3 augeas-libs
    sudo python3 -m venv /opt/certbot/
    sudo /opt/certbot/bin/pip install --upgrade pip
    sudo /opt/certbot/bin/pip install certbot certbot-apache
    sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
  ```

16. Obtain your SSL certificate by running Certbot and following the interactive prompts.

   ```bash
    sudo certbot --apache
  ```


Remember to enter your email, agree to the terms and conditions, and select the option to force HTTPS for better security.

To verify your SSL setup: 

simply visit your site at 'https://yourdomain.com' in your browser. If configured correctly, a padlock icon should appear in the address bar, indicating a secure connection.











## You've successfully installed SSL on Amazon Linux 2023 with Apache and PHP packages.

![App Screenshot](https://s3.amazonaws.com/mejba.me/Successfully+set+up+SSL+on+Amazon+Linux+2023+with+Apache+and+PHP+packages.png)


## Support

For support - https://www.mejba.me/

