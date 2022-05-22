# Hosting-MERN-application-on-AWS-cloud

## Perquisites
- Aws account
- Putty / aws console
- Github Account & Git
- Visual studio code or any ide
- MongoDB Atlas
## Procedure
### Virtual server setup EC2
1.	Choose a machine image
2.	Choose a Instance type
3.	Confirgure the instance
4.	Add storage 
5.	Add tags 
6.	Configure security group 
7.	Review and launch

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(224).png)

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(226).png)

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(227).png)

### Connect to EC2 using aws console or putty
**In terminal of EC2 run the following commands**
```
Sudo yum update -y 
```
### MongoDB , NodeJS and Nginx Setup
#### NODE & NPM
**In terminal of EC2 run the following commands**
```
add nodejs 10 ppa (personal package archive) from nodesource
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -

install nodejs and npm
sudo apt-get install -y nodejs

```

#### MONGODB(optional if you want to use mongodb on you own server [I am using MONGODB Atlas personally ])
**In terminal of EC2 run the following commands**
```
import mongodb 4.0 public gpg key
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4

create the /etc/apt/sources.list.d/mongodb-org-4.0.list file for mongodb
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list

reload local package database
sudo apt-get update

install the latest version of mongodb
sudo apt-get install -y mongodb-org

start mongodb
sudo systemctl start mongod

set mongodb to start automatically on system startup
sudo systemctl enable mongod

```

#### PM2
**In terminal of EC2 run the following commands**
```
install pm2 with npm
sudo npm install -g pm2

set pm2 to start automatically on system startup
sudo pm2 startup systemd

```

#### NGINX
**In terminal of EC2 run the following commands**
```
install nginx
sudo apt-get install -y nginx

```

#### UFW (FIREWALL)
**In terminal of EC2 run the following commands**
```
allow ssh connections through firewall
sudo ufw allow OpenSSH

allow http & https through firewall
sudo ufw allow 'Nginx Full'

enable firewall
sudo ufw --force enable

```

### Setting up MONGODB Atlas cluster
***create project 
give name to project and next
create database***

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(231).png)

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(239).png)

### Connect mongo db cluster in the application project …
### Install git and setup app on server

```
sudo apt install git
cd /opt/
sudo mkdir mernapp
cd mernapp
git clone <your app project here>
```

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(233).png)

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(234).png)

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(235).png)

### Configure the application with reverse Proxy
***1. Setup the config file***
```
cd project_reponame
cd backend
sudo npm install
sudo npm audit fix –force
cd client
sudo npm install
sudo npm audit fix –force
sudo npm run build
```
![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(236).png)

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(237).png)

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(238).png)

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(239).png)

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(240).png)

```
sudo nano /etc/nginx/sites-available/default
```
```
server {
 listen 80 default_server;
 listen [::]:80 default_server;
 root /var/www/html;
 index index.html index.htm index.nginx-debian.html;
 server_name _;
 location / {
 root /opt/mernapp/mern_project_aws/backend/client/build;
 # as directory, then fall back to displaying a 404.
 try_files $uri /index.html;
 }
 location /api/ {
 proxy_pass http://localhost:5000/;
 }
}

```

***Then save it and exit***
```
sudo service nginx restart
```

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(243).png)

### Verify the application access on the server

![Screenshot](https://github.com/prakash02dec/Hosting-dynamic-application-AWS-RDS-/blob/00f33aa11819d2e6059b04f62660e419f754efa4/images/Screenshot%20(242).png)
