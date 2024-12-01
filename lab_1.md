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

