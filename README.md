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

## 5. if you are successfull sample output below:  
![alt text][logo]

[logo]: https://github.com/ohmcodes/AWS-EC2-setup-and-nginx-wordpress-postgresql-configurations/blob/master/default_apache.png?raw=true


## 6. Pointing your Project File
the default directory for web (/var/www/html/)


## 7. Configure Nginx
sudo nano /etc/nginx/sites-available/wordpress

```
server {
    listen 80;
    listen [::]:80;
    root /var/www/html/<![#f03c15]ProjectFILE>;
    index  index.php index.html index.htm;
    server_name  <![#f03c15]AWS EC2 Public DNS>;

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
