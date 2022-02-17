# Ansible

## Installation

### Ansible master

```bash
#sudo su -
#vi /etc/hostname 
hostname ansible-control-node
useradd ansadmin
passwd ansadmin
visudo
  add ansadmin
ssh-keygen
vi /etc/ssh/sshd_config
    passwordauthentication yes
service sshd restart

#ubuntu
useradd -m -d /home/ansadmin ansadmin
# visudo ansadmin    ALL=(ALL)   NOPASSWD: ALL

#
sudo yum update -y
# yum install python3-pip -y
pip3 install ansible
ansible --version
ansible-config init --disabled -t all > ansible.cfg

ansible all -m setup
ssh-copy-id 172.32.65.89
ssh 172.32.65.89
```

## ad-hoc

```bash
ansible all -m ping -i hosts
ansible all -m command -a "who" -i hosts
ansible all -m stat -a "path=/etc/hosts" -i hosts
ansible all -m yum -a "name=git" -b -i hosts
ansible all -m user -a "name="john" -b i hosts
ansible all -m setup i hosts
```

## Links

Ansible installation guide - <https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html>

Ansible installation guide 2 - <https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04>

Ansible tower AWX v17 - <https://www.linuxtechi.com/install-ansible-awx-on-ubuntu/>

Ansible tower AWX v20 - <https://computingforgeeks.com/how-to-install-ansible-awx-on-ubuntu-linux/>
