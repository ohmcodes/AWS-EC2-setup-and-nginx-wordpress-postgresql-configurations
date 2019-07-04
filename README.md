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


## 6. Installing Wordpress and Pointing your Project File
```
sudo cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
```
### Then unzip the package using
```
sudo tar -xzvf latest.tar.gz
```
### Remove the zip file
```
sudo rm -rf latest.tar.gz
```


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
        #fastcgi_pass             unix:/var/run/php5-fpm.sock; #Ubuntu 14
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
sudo apt-get install php7.0-cli php7.0-cgi php7.0-fpm php-dom php7.0-curl php7.0-zip
```

### If you wish to edit php.ini
```
sudo nano /etc/php/7.0/fpm/php.ini
sudo systemctl restart php7.0-fpm
```

### Setup php mail()
```
sudo apt-get install sendmail
sudo sendmailconfig ~yes to all
sudo service nginx restart
```

### Modify restriction to wp forlders
```
chmod 777 to /wp-content /themes /plugins folders
```

## 14. Installing PG4WP (postgresql for Wordpress)
### Download and configure the Fork version of WP4PG in your WordPress directory
```
cd wp-content // go to wp-content
sudo git clone https://github.com/kevinoid/postgresql-for-wordpress.git
sudo mv postgresql-for-wordpress/pg4wp pg4wp
sudo rm -rf postgresql-for-wordpress
sudo cp pg4wp/db.php db.php
```

## 15. Encountered error
### Your PHP installation appears to be missing the PostgreSQL extension which is required by WordPress with PG4WP.

### Pecl PDO package is now deprecated. By the way the debian package php5-pgsql now includes both the regular and the PDO driver:
```
sudo apt-get install php-pgsql
```

## 16. Configure Wordpress
### Now that Nginx is configured, run the commands below to create WordPress wp-config.php file.
```
sudo mv /var/www/html/<ProjectFILE>/wp-config-sample.php /var/www/html/<ProjectFILE>/wp-config.php
```

## 17. Then run the commands below to open WordPress configuration file.
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

## Installing template manually (E.G: Avada template)
Download Avada and extract it then compress for sending it out to server (Avada.zip)
upload file from local to server via secured scp
```
scp -i "<keypair>.pem" "<filename to be upload>" <user>@<ec2 instance public dns>
```

## Download unzip
```
apt-get install unzip
unzip <filename>.zip if zip file
```

## Move the template
```
mv <filename> <ProjectPath>/wp-content/themes/
```

## 18. DONE!
Enjoy
