# iVolve-training

# Lab 1: Configure User and Group for Nginx Installation Without Sudo Password

In this lab, you will:
1. Create a user named `test`.
2. Create a group named `ivolve`.
3. Add the `test` user to the `ivolve` group.
4. Configure the `test` user to install and manage Nginx without being prompted for a sudo password.

## Steps

### 1. Create the `test` user
```bash
sudo adduser test
```

### 2. Create the `ivolve` group
```bash
sudo groupadd ivolve
```

### 3. Add `test` to the `ivolve` group
```bash
sudo usermod -aG ivolve test
```

### 4. Modify sudoers file to allow passwordless Nginx installation
```bash
sudo visudo
test ALL=NOPASSWD: /usr/bin/apt-get install nginx, /usr/sbin/service nginx *
```

### 5. Install Nginx as `test` user
```bash
sudo -i -u test
sudo apt-get install nginx
```

