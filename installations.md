# SETUP - COMMANDS

## UBUNTU- 20.04

### Docker

```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce

sudo apt install docker-ce -y
sudo chmod 666 /var/run/docker.sock
sudo systemctl enable docker
# docker run -d -p 5000:5000 --restart unless-stopped --name flask-app akhilmovva/flask-image
```

### MySQL

#### Server

```bash
sudo apt update
sudo apt install mysql-server mysql-client -y
sudo systemctl start mysql
sudo systemctl enable mysql
 
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
# edit the file by changing the bind-address to 0.0.0.0
sudo systemctl restart mysql
sudo mysql_secure_installation
 
sudo mysql -u root -p
# For connecting as created user
mysql> SET GLOBAL validate_password.policy = 0;
mysql> CREATE USER  'akhil'@'%' IDENTIFIED BY 'Avengers123';
mysql> GRANT ALL PRIVILEGES ON  *.* to 'akhil'@'%';
mysql> ALTER USER 'akhil'@'%' IDENTIFIED WITH mysql_native_password BY 'Avengers123';
 
# For connecting as root user
mysql> SHOW DATABASES;
mysql> USE mysql;
mysql> SELECT host FROM user WHERE user = root;
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'akhilmovva' WITH GRANT OPTION;
```

#### Host / Node

```bash
sudo apt update -y
sudo apt install mysql-client -y
mysql -h <hostname> -u <username> -p
```

#### Creating the database

```bash
sudo mysql -u root -p
mysql> CREATE DATABASE flaskapp;
mysql> USE flaskapp;
mysql> CREATE TABLE users(name varchar(20), email varchar(40));
mysql> INSERT INTO users VALUES('akhil','akhil@gmail.com');
```

### Nginx

```bash
sudo apt search
sudo apt update
sudo apt install nginx
 
sudo ufw app list
sudo ufw enable
sudo ufw allow 'Nginx HTTP'
sudo ufw status
 
systemctl status nginx
```

### Jenkins

```bash
sudo apt update
sudo apt install default-jre -y
sudo apt install default-jdk -y
 
sudo wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
 
sudo apt update
sudo apt install jenkins -y
 
sudo systemctl start jenkins
sudo systemctl status jenkins
sudo ufw enable -y
sudo ufw allow 8080
sudo ufw status
```

## USEFUL COMMANDS

```bash
# PYTHON PIP 3 
sudo apt update -y
sudo apt install python3-pip -y
sudo apt-get install python3.8
# FLASK
pip3 install flask
sudo apt-get install libmysqlclient-dev -y
pip3 install flask-mysqldb
```

## CENTOS 7

### MySQL - 7

```bash
sudo yum update
#https://dev.mysql.com/downloads/repo/yum/
sudo wget https://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm
#md5sum mysql80-community-release-el7-5.noarch.rpm
sudo rpm -Uvh mysql80-community-release-el7-5.noarch.rpm
 
sudo yum install mysql-server
 
sudo systemctl start mysqld
sudo systemctl status mysqld
```

### Nginx - 7

```bash
sudo yum search
Sudo yum update
sudo yum install epel-release
sudo yum install nginx
 
sudo systemctl start nginx
sudo systemctl status nginx
```

### Jenkins - 7

```bash
sudo yum install java-11-openjdk.x86_64 -y
 
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
 
sudo yum install epel-release # repository that provides 'daemonize'
sudo yum install jenkins
 
sudo systemctl start jenkins
sudo systemctl status jenkins
 
sudo systemctl enable jenkins
```

#### Jenkins EKS integration

```bash
# AWS CLI
sudo apt install aws cli

# kubectl
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl 
sudo mv ./kubectl /usr/local/bin

# eksctl - Optional
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin 
eksctl version

# update kubeconfig for the required cluster
aws eks update-kubeconfig --name eksdemo1
```
