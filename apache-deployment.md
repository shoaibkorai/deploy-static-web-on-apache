# Deploying Your Portfolio Website on Apache

This guide will walk you through the steps to deploy your portfolio website on Apache, using `portfolio` as the website directory and `portfolio.conf` as the configuration file. The domain for the website will be set as `myportfolio.com`.

## Prerequisites
- Ubuntu operating system
- Basic knowledge of terminal commands
- Apache installed on your system
- Your website files in a directory named `portfolio`

## Steps

### Step 1: Install Apache
First, ensure that Apache is installed on your Ubuntu system. If it's not already installed, you can install it using the following commands:

```sh
sudo apt update
sudo apt install apache2
```

### Step 2: Create the Website Directory
Assuming your website directory is named `portfolio` and is located in your home directory, copy it to the Apache web directory:

```sh
sudo cp -r ~/portfolio /var/www/html/
```

### Step 3: Create the Apache Configuration File
Next, create a new configuration file for your website in the Apache sites-available directory:

```sh
sudo vi /etc/apache2/sites-available/portfolio.conf
```

Add the following content to the file:

```apache
<VirtualHost *:80>
    ServerAdmin webmaster@myportfolio.com
    ServerName myportfolio.com
    ServerAlias www.myportfolio.com

    DocumentRoot /var/www/html/portfolio
    <Directory /var/www/html/portfolio>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/portfolio_error.log
    CustomLog ${APACHE_LOG_DIR}/portfolio_access.log combined
</VirtualHost>
```

This configuration file sets up a virtual host for your website, specifying the document root and log file locations.

### Step 4: Enable the Configuration
Enable the new site configuration and disable the default site configuration if necessary:

```sh
sudo a2ensite portfolio.conf
sudo a2dissite 000-default.conf
```



### Step 5: Restart Apache
Restart Apache to apply the changes:

```sh
sudo systemctl restart apache2
```

### Step 6: Update Your Hosts File (For Local Testing)
If you are testing locally and don't have a domain name yet, you can update your `/etc/hosts` file to map `myportfolio.com` to `localhost`:

```sh
sudo vi /etc/hosts
```

Add the following line:

```sh
127.0.0.1   myportfolio.com
```

### Step 7: DNS Configuration (For Live Domain)
If you have a domain name, update the DNS settings of your domain registrar to point to your server's IP address. This typically involves setting an A record for `myportfolio.com` and `www.myportfolio.com` to your server's IP.

### Step 8: Test Your Website
Finally, open a web browser and navigate to `http://myportfolio.com` to verify that your website is up and running.

## Conclusion
By following these steps, you should have your portfolio website successfully deployed on Apache with the domain `myportfolio.com`. If you encounter any issues, make sure to check the Apache error logs for more information:

```sh
sudo tail -f /var/log/apache2/portfolio_error.log
```
