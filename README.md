# AWS-EC2-setup-and-nginx-wordpress-postgresql-configurations



## 1. Initialization: EC2 Setup  
...*Log in to AWS
...*Go to AWS Console
...*On Instances, Launch Instance
...*Choose (Free Tier) Ubuntu Server 16.04 LTS (HVM)
...*Choose Free Tier Instance Type
...*Skip to Step 6 - Configure Security Group
...*Add Rule HTTP
...*Review and Launch , Ignore warning, Launch
...*Choose a key pair or create a new one your choice
...*EC2 Setup - Done *

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
