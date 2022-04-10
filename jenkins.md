# Jenkins master-master in same region

jenkins on redhat linux

sudo yum install wget
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade

sudo yum install java-11-openjdk

1.sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
2.sudo dnf repolist
3.sudo dnf install daemonize
4.sudo yum install jenkins
5.sudo systemctl start jenkins
6.sudo systemctl enable jenkins
7.systemctl status jenkins

sudo systemctl start nfs-server
sudo systemctl status nfs-server

sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-096790c153ea27e3f.efs.us-east-1.amazonaws.com:/ /var/lib/jenkins/jobs

sudo chown -R jenkins:jenkins /var/lib/jenkins/jobs

systemctl restart jenkins

Crumb code: 
################################
#!/bin/bash
crumb_id=$(curl -s 'http://localhost:8080/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,":",//crumb)' -u admin:admin)
curl -s -XPOST 'http://localhost:8080/reload' -u admin:admin -H "$crumb_id"

crumb_id=$(curl -s 'http://localhost:8080/crumbIssuer/api/xml?xpath=concat%28%2F%2FcrumbRequestField,":",//crumb)' -u admin:admin)



enable proxy compatibility - CSRF Protection - global security - crumb issuer

cd ~
vi reload.sh
chmod +x reload.sh

vi /etc/cron.d/jenkinsreload
	*/1 * * * * root /bin/bash /root/reload.sh  - every min

systemctl status crond


sudo su -
yum install haproxy

vi /etc/haproxy/haproxy.cfg


HA Proxy:
######################

defaults
  log  global
  maxconn  2000
  mode  http
  option  redispatch
  option  forwardfor
  option  http-server-close
  retries  3
  timeout  http-request 10s
  timeout  queue 1m
  timeout  connect 10s
  timeout  client 1m
  timeout  server 1m
  timeout  check 10s

frontend ft_jenkins
  bind *:80
  default_backend  bk_jenkins
  reqadd  X-Forwarded-Proto:\ http

backend bk_jenkins
  server <hostname> 172.31.30.94:8080 check
  server <hostname> 172.31.24.10:8080 check backup

systemctl start haproxy
systemctl status haproxy


nslookup myrabbitp-server-0.myrabbit-nodes.default.svc.cluster.local
nslookup myrabbit-server-0.myrabbit-nodes.default.svc.cluster.local