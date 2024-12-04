# Lab 11: Set up Ansible dynamic inventories to automatically discover and manage infrastructure. Use Ansible Galaxy role to install Apache.


ansible.cfg
```cfg
[defaults]
inventory = ./aws_ec2.yml
host_key_checking = False
remote_user = ubuntu
private_key_file = ./ubuntu.pem
#remote_user = ec2-user
#private_key_file = ./amazon.pem
```


aws_ec2.yml
```yaml
plugin: aws_ec2
regions:
  - us-east-1  # adjust region as needed
filters:
  tag:Environment: ivolve  # adjust tags as needed
keyed_groups:
  - prefix: tag
    key: tags
compose:
  ansible_host: public_ip_address
```

webserver.yml
```yaml
---
- hosts: tag_Role_webserver  # targets hosts tagged with Role:webserver
  become: yes
  vars:
   # ansible_ssh_private_key_file: ./ubuntu.pem
   # ansible_user: ubuntu
    apache_listen_port: 80
    apache_vhosts:
      - servername: "example.com"
        documentroot: "/var/www/html"

  roles:
    - geerlingguy.apache
```
