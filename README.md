Ansible-vagrant-HAProxy-nginx-PHP-redis-MySQL
=============================================

A playbook for provisioning vagrants with HAProxy, nginx, php-fpm, redis, MySQL.
A simple PHP website is served.

## Requirements

- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](http://www.vagrantup.com/downloads.html)
- [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html)

## Overview

It will create 1 vagrant with HAProxy, 2 with nginx, php-fpm, redis and MySQL  
You can customize predefined private IPs in the Vagrantfile.
Tested on CentOS 7.4.

## Usage

- Test on a local machine.

```bash
$ vagrant up
```
- For live testing, use comprigo-tzidanic.tk


## Explanations

### HAProxy

- SSL termination on frontend
```
option forwardfor
        reqadd X-Forwarded-Proto:\ https
        reqadd X-Forwarded-Port:\ 443
```

- Create SSL with LetsEncrypt
- Generate a DH key and enable dhparam
```
certbot certonly --standalone --preferred-challenges http --http-01-port 80 -d comprigo-tzidanic.tk -d www.comprigo-tzidanic.tk

cat /etc/letsencrypt/live/comprigo-tzidanic.tk/fullchain.pem
cat /etc/letsencrypt/live/comprigo-tzidanic.tk/privkey.pem
openssl dhparam -out /etc/ssl/dh-param.txt 2048
```
The 3 files were saved in /etc/ssl/comprigo-tzidanic.tk.pem

- Use TLS only
- Enable HSTS
```
rspadd  Strict-Transport-Security:\ max-age=15768000
```
- Enable OCSP Stapling
```
stats socket /var/run/haproxy.sock mode 600 level admin
```

- Send traffic to nginx SSL backend
```
server nginx1 10.1.1.41:443 check ssl verify none
server nginx2 10.1.1.42:443 check ssl verify none backup
```

### nginx

- Enable HTTPS
- Enable Nginx Microcaching and make sure Cache is not enabled in Admin Area and for logged in users
```
proxy_cache_path /var/cache/nginx levels=1:2 
		keys_zone=microcache:5m max_size=1000m;
```

### php-fpm

- For process management:
Dynamic was chosen as a compromise between speed and memory usage, although on this test server, ondemand would also work as this is a low traffic server.
Static would be a good choice on a high traffic server, that needs to serve PHP very quickly and can dedicate plenty of memory to PHP-FPM.
- Listen directive:
Here socket was good enough, as there is only one tiny script, low demand with locally installed nginx. 
Socket should be a bit faster than TCP as networking is not involved. TCP would be a better choice for a busy server.


### Redis
