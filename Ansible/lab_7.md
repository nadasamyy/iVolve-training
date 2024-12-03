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

---

## Test Ansible ad-hoc commands

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

### 3. Run some ad-hoc commands
> **Note:**
> Using with sudo (become):
```bash
# Add -b or --become to use sudo
ansible webservers -b -m shell -a "tail /var/log/messages"
```

#### 1. Basic ping test:
```bash
# Test ping
ansible webservers -m ping
```

#### 2. System information:
```bash
# Get system facts
ansible webservers -m setup

# Get memory information
ansible webservers -m shell -a "free -m"

# Get disk space
ansible webservers -m shell -a "df -h"

# Get OS information
ansible webservers -m shell -a "cat /etc/os-release"
```

#### 3. Check system uptime and load:
```bash
ansible webservers -m shell -a "uptime"
```

#### 4. Process management:
```bash
# List running processes
ansible webservers -m shell -a "ps aux"

# Check specific service status
ansible webservers -m service -a "name=sshd state=started"
```

#### 5. Package management:
```bash
# Check if a package is installed
ansible webservers -m yum -a "name=httpd state=present"

# List all installed packages
ansible webservers -m shell -a "yum list installed"
```

#### 6. File operations:
```bash
# Check file existence
ansible webservers -m stat -a "path=/etc/passwd"

# Get file content
ansible webservers -m shell -a "cat /etc/hostname"

# Check file permissions
ansible webservers -m shell -a "ls -l /etc/passwd"
```

#### 7. Network checks:
```bash
# Check listening ports
ansible webservers -m shell -a "netstat -tuln"

# Check DNS resolution
ansible webservers -m shell -a "nslookup google.com"

# Check connectivity to a specific host
ansible webservers -m shell -a "ping -c 4 8.8.8.8"
```

#### 8. User management:
```bash
# List all users
ansible webservers -m shell -a "cat /etc/passwd"

# Check specific user
ansible webservers -m shell -a "id ec2-user"
```

#### 9. Memory and CPU information:
```bash
# Check memory stats
ansible webservers -m shell -a "vmstat"

# Check CPU info
ansible webservers -m shell -a "lscpu"
```

#### 10. Security checks:
```bash
# Check SELinux status
ansible webservers -m shell -a "getenforce"

# List sudo privileges
ansible webservers -m shell -a "sudo -l"
```
