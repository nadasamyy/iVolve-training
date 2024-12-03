# Lab 7: Install and configure Ansible Automation Platform on control nodes, create inventories of a managed host, and then perform ad-hoc commands to check functionality.

## install ansible on ubuntu

### 1. Update your system packages:
```bash
sudo apt-get update
sudo apt-get upgrade -y
```

### 2. Install required dependencies:
```bash
sudo apt-get install -y software-properties-common
```

### 3. Add Ansible repository and refresh package list:
```bash
sudo apt-add-repository --yes --update ppa:ansible/ansible
```

### 4. Install Ansible:
```bash
sudo apt-get install -y ansible

#Verify the installation:
ansible --version
```

## Test Playbook

### 1. Create `ansible.cfg`
```bash
nano ansible.cfg
```
add the following:
```
[defaults]
inventory = ./inventory
host_key_checking = False
remote_user = ec2-user
private_key_file = ./amazon.pem
```

### 2. Create `inventory`
```bash
nano inventory
```
add the following:
```
[webservers]
ec2-54-161-238-251.compute-1.amazonaws.com
```
