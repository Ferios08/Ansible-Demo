# ⚙️ Ansible Example 🛠️

## 1. Install Ansible

### If you are using Ubuntu / Debian

```sh
sudo apt update
sudo apt install ansible
```

### If you are using CentOS / Redhat

```sh
sudo yum install epel-release
sudo yum install ansible
```

## 2. Generate a new SSH KEY

```sh
ssh-keygen -t rsa -f $HOME/.ssh/id_rsa  -q -P ""  <<< y
```

## 3. Copy SSH Key to remote servers
```sh
ssh-copy-id remote_user@remote_machine
```

## 4. Copy SSH Key to remote servers
```sh
ssh-copy-id remote_user@remote_machine
```
## Build your inventory file:
edit `inventory.yml` file:

```yaml
web:
  hosts:
    web1:
      ansible_connection: ssh # can be local in case host is localhost
      ansible_host: 1.1.1.1
    web2:
      ansible_connection: ssh # can be local in case host is localhost
      ansible_host: 2.2.2.2
    # You can add as many machines as you want

  vars:
    domain: www.helloworld.com # a fully qualifed domain name, or use nip.io to transfer your IP into a DN

db:
  hosts:
    db1:
      ansible_connection: ssh # can be local in case host is localhost
      ansible_host: 3.3.3.3
  
  vars:
    mysql_root_password: password

all:
  vars:
    ansible_python_interpreter: /usr/bin/python3
    ansible_ssh_private_key_file: ~/.ssh/id_rsa
    ansible_user: firas
    ansible_sudo_pass: pass
```
you can check `ansible.cfg` for variables that you can change/ update:
```sh
[defaults]
remote_tmp= /tmp/.ansible-${USER}/tmp
```

## 5. Deploy 🚀 your app:

###   5.1. Nginx App

```sh
# To install Nginx
ansible-playbook -i inventory.yml  nginx/nginx_install.yml
# To enable the new domain name
ansible-playbook -i inventory.yml  nginx/nginx_sync_domain.yml
# To secure the new domain name with SSL 
ansible-playbook -i inventory.yml  nginx/nginx_secure_domain_https.yml
```

###   5.2. MySQL Database

```sh
# To install MySQL
ansible-playbook -i inventory.yml  mysql/mysql_install.yml
```

###   5.2. Docker

```sh
# To install Docker
ansible-playbook -i inventory.yml docker/docker_install.yml
# To Create a Docker container
ansible-playbook -i inventory.yml docker/docker_install.yml
# To delete the Docker container
ansible-playbook -i inventory.yml docker/docker_install.yml
```