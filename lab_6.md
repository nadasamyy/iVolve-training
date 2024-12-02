# Lab 6: Generate public and private keys and enable SSH from your machine to another VM using the key. Configure SSH to just run ‘ssh ivolve’ without specify username, IP and key in the command.

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
