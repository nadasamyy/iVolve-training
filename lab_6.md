# Lab 6: Generate public and private keys and enable SSH from your machine to another VM using the key. Configure SSH to just run ‘ssh amazonlinux’ without specify username, IP and key in the command.

## prequisites

On Ubuntu EC2, you need to add the EC2 Amazon Linux private key to establish the initial connection for ssh-copy-id. Here's how:

1. First, create the .ssh directory if it doesn't exist:
```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

2. Add the private key (assuming you have the .pem file):
```bash
# Copy your .pem file content to a new private key file
nano ~/.ssh/amazonlinux.pem

# Set correct permissions
chmod 600 ~/.ssh/amazonlinux.pem
```

3. Configure SSH to use this key:
```bash
# Edit or create SSH config
nano ~/.ssh/config

# Add these lines
Host amazonlinux
    HostName <amazon-linux-ip>
    User ec2-user
    IdentityFile ~/.ssh/amazonlinux.pem
    StrictHostKeyChecking accept-new
```

4. connect
```bash
ssh amazonlinux
```

## Steps

### 1. Generate SSH Key Pair:
```bash
# Generate RSA key pair
ssh-keygen -t rsa -b 4096

# Press Enter to save in default location (~/.ssh/id_rsa)
# Optionally set a passphrase or press Enter for no passphrase
```

### 2. Copy Public Key to Remote VM:
```bash
# Replace username and remote_ip with your values
ssh-copy-id username@remote_ip
```

### 3. Configure SSH Config File:
```bash
# Create or edit SSH config file
nano ~/.ssh/config

# Add these lines
Host ivolve
    HostName remote_ip
    User username
    IdentityFile ~/.ssh/id_rsa
    ServerAliveInterval 120
```

### 4. Set Proper Permissions:
```bash
# Set correct permissions for SSH directory and files
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
```

### 5. Test the Connection:
```bash
# Now you can simply use
ssh ivolve
```
