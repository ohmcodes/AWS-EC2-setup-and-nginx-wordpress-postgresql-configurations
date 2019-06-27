# AWS-EC2-setup-and-nginx-wordpress-postgresql-configurations



## 1. Initialization: EC2 Setup  
* Log in to AWS  
* Go to AWS Console  
* On Instances, Launch Instance  
* Choose (Free Tier) Ubuntu Server 16.04 LTS (HVM)  
* Choose Free Tier Instance Type  
* Skip to Step 6 - Configure Security Group  
* Add Rule HTTP  
* Review and Launch , Ignore warning, Launch  
* Choose a key pair or create a new one your choice  
* EC2 Setup - Done *  

## 2. AWS: Ubuntu Server Setup  
```
ssh to the server: example AWS EC2 Public DNS = ec2-12-123-123-123.us-west-1.compute.amazonaws.com
ssh -i "keypair.pem" ubuntu@<AWS EC2 Public DNS>
```

## 3. Update the server and install nginx  
```
sudo apt-get update
sudo apt-get install nginx
```

## 4. Start Nginx  
```
sudo systemctl stop nginx.service
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
```

## 5. if you are successfull sample output below:  open browser and visit IPv4 Public IP in EC2
![alt text][logo]

[logo]: https://github.com/ohmcodes/AWS-EC2-setup-and-nginx-wordpress-postgresql-configurations/blob/master/default_apache.png?raw=true


## 6. Pointing your Project File
the default directory for web (/var/www/html/)


## 7. Configure Nginx
change values insde <>
```
sudo nano /etc/nginx/sites-available/<SiteName>
```
Paste Code Below:  
```
server {
    listen 80;
    listen [::]:80;
    root /var/www/html/<ProjectFILE>;
    index  index.php index.html index.htm;
    server_name  <AWS EC2 Public DNS>;

    location / {
    	try_files $uri $uri/ /index.php?$args;        
    }

    location ~ \.php$ {
    	fastcgi_split_path_info  ^(.+\.php)(/.+)$;
    	fastcgi_index            index.php;
    	fastcgi_pass             unix:/var/run/php/php7.1-fpm.sock; #Ubuntu 17.10
  	#fastcgi_pass             unix:/var/run/php/php7.0-fpm.sock; #Ubuntu 17.04
    	include                  fastcgi_params;
    	fastcgi_param   PATH_INFO       $fastcgi_path_info;
    	fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

}
```

## 8. Enable the WordPress Site
```
sudo ln -s /etc/nginx/sites-available/<SiteName> /etc/nginx/sites-enabled/
```

## 9. To avoid using default site
```
sudo rm /etc/nginx/sites-enabled/default
```

## 10. Restart Nginx
```
sudo systemctl restart nginx.service
```

## 11. Install Postgresql visit guide
https://github.com/ohmcodes/Install-Postgresql-in-Ubuntu-and-Config

## 12. Install PHP-FPM and Related Modules
```
sudo apt install php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-mcrypt php-ldap php-zip php-curl
```

## 13.other package might need
for ubuntu 16  
### Install nginx, PHP for Processing, and Required PackagesPermalink
```
sudo apt-get install php7.0-cli php7.0-cgi php7.0-fpm
```

## 14. Encountered error
### Your PHP installation appears to be missing the PostgreSQL extension which is required by WordPress with PG4WP.

### Pecl PDO package is now deprecated. By the way the debian package php5-pgsql now includes both the regular and the PDO driver:
```
sudo apt-get install php-pgsql
```

## 15. Configure Wordpress
### Now that Nginx is configured, run the commands below to create WordPress wp-config.php file.
```
sudo mv /var/www/html/<ProjectFILE>/wp-config-sample.php /var/www/html/<ProjectFILE>/wp-config.php
```

## 16. Then run the commands below to open WordPress configuration file.
```
sudo nano /var/www/html/<ProjectFILE>/wp-config.php


// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', '<YOUR DB NAME>');

/** MySQL database username */
define('DB_USER', '<YOUR DB USERNAME>');

/** MySQL database password */
define('DB_PASSWORD', '<YOUR DB PASSWORD>');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');
```


## 17. DONE!
Enjoy
