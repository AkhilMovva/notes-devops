# Master Worker

## MySQL

### Master

```bash
# master node -  172.31.82.248
# slave node  - 172.31.91.129
# master node 2 - 172.31.80.98

sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

# bind-address = 192.168.1.51
# server-id = 1
# log_bin = mysql-bin
# binlog_do_db = flaskapp

sudo systemctl restart mysql

sudo mysql -u root -p

mysql> 
SHOW MASTER STATUS\G

GRANT REPLICATION SLAVE ON *.* TO 'akhil'@'%';
FLUSH PRIVILEGES;
FLUSH TABLES WITH READ LOCK;
SHOW MASTER STATUS;
# 1    mysql-bin.000001 |     1800 | flaskapp 
# 2    mysql-bin.000001 |     7156 | flaskapp
UNLOCK TABLES;
```

### Worker

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf

# bind-address = 192.168.1.51
# server-id = 1
# log_bin = mysql-bin
# binlog_do_db = flaskapp
# relay_log = /var/log/mysql/mysql-relay-bin.log

sudo systemctl restart mysql

sudo mysql -u root -p

mysql> 
STOP SLAVE;

CHANGE MASTER TO
MASTER_HOST='172.31.82.248',
MASTER_USER='akhil',
MASTER_PASSWORD='Avengers123',
MASTER_LOG_FILE='mysql-bin.000001',
MASTER_LOG_POS=1800;

START SLAVE;
SHOW SLAVE STATUS\G;
```

## MongoDB

### Primary

```bash
# 172.31.18.91    ip-172.31.18.91
# 172.31.85.97    ip-172-31-85-97
# <ip3>               <ip3>

sudo nano /etc/hosts

sudo apt install ufw
sudo ufw enable
sudo ufw allow 27017
sudo ufw allow 22
sudo ufw status verbose

sudo nano /etc/mongod.conf
#  bindIp: mongohostip
#replication:
#  replSetName: "mongorep"


sudo systemctl restart mongod.service
sudo systemctl status mongod
mongo 172.31.18.91 
rs.initiate()
rs.add("172.31.85.97")
rs.add("192.168.152.142")
rs.status()

```

### Secondary

```bash
# repeat until mongo <ip> from the primary server
rs.secondaryOk()
```

### Testing with data

```bash
show dbs
use test

db.eclhur.insert({"name":"a2"})
db.eclhur.find()
```

## Links

MySQL master-master - <https://www.digitalocean.com/community/tutorials/how-to-set-up-mysql-master-master-replication>

MongoDB Installation - <https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/>

MongoDB replication - <https://www.r2schools.com/mongodb-replication-setup-step-by-step-on-linux/>
