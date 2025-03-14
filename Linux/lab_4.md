# Lab 4: Demonstrate the differences between using the hosts file and DNS for URL resolution. Modify the hosts file to resolve a URL to a specific IP address, then configure BIND9 as a DNS solution to resolve wildcard subdomains and verify resolution using dig or nslookup (e.g. memo.com --> 1.1.1.1).

## Steps

### 1. Install BIND9
```bash
# Update system
sudo apt-get update

# Install BIND9
sudo apt-get install -y bind9 bind9utils bind9-doc
```

### 2. Configure Options
Edit `named.conf.options`
```bash
sudo nano /etc/bind/named.conf.options
```
Add this configuration:
```
options {
        directory "/var/cache/bind";

        forwarders {
                8.8.8.8;
                8.8.4.4;
        };

        listen-on { any; };
        listen-on-v6 { any; };
        allow-query { any; };

        dnssec-validation auto;
        auth-nxdomain no;
};
```

### 3. Configure Local Zone
Edit `named.conf.local`
```bash
sudo nano /etc/bind/named.conf.local
```
Add this configuration:
```
zone "memo.com" {
        type master;
        file "/etc/bind/zones/db.memo.com";
};
```

### 4. Create Zone Directory and File
```bash
# Create zones directory
sudo mkdir /etc/bind/zones

# Create zone file
sudo nano /etc/bind/zones/db.memo.com
```
Add this content:
```
$TTL    604800
@       IN      SOA     memo.com. root.memo.com. (
                     2023121001         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

@       IN      NS      ns.memo.com.
@       IN      A       1.1.1.1
ns      IN      A       1.1.1.1
www     IN      A       1.1.1.1
*       IN      A       1.1.1.1
```

### 5. Set Permissions
```bash
# Set ownership
sudo chown -R bind:bind /etc/bind/zones

# Set permissions
sudo chmod 760 /etc/bind/zones
```

### 6. Check Configuration
```bash
# Check named.conf syntax
sudo named-checkconf

# Check zone file syntax
sudo named-checkzone memo.com /etc/bind/zones/db.memo.com
```

### 7. Start and Enable BIND9
```bash
# Start BIND9
sudo systemctl start bind9

# Enable on boot
sudo systemctl enable bind9

# Check status
sudo systemctl status bind9
```

### 8. Test Configuration
> [!NOTE]
> dig @server domain_name , @server: Specifies which DNS server to query  
> @localhost:  Query your local DNS server (BIND9 on your machine)  
> If you don't specify @server, dig will use the default nameserver in `/etc/resolv.conf`  
```bash
# Test using dig, 
dig @localhost memo.com

# Test using nslookup
nslookup memo.com localhost
```

## Troubleshooting Commands
```bash
# Check logs
sudo tail -f /var/log/syslog | grep named

# Check if BIND9 is listening
sudo netstat -tulpn | grep named

# Restart BIND9 if needed
sudo systemctl restart bind9
```

## Important Notes:
Make sure your EC2 security group has these ports open:
- TCP port 53
- UDP port 53

as DNS uses both TCP and UDP port 53, so both need to be open in your security group.

## Test
```
# Test using localhost
dig @localhost memo.com

# Test using EC2's private IP
dig @private_ip memo.com

# Test using EC2's public IP
dig @public_ip memo.com
```

---

## 📄 License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## ✍️ Author
**King Memo**

## 🙏 Thank You!
Thank you for using this project. Your support and feedback are greatly appreciated!
