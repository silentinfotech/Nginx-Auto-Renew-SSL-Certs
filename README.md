Nginx Auto Renew SSL Certificates
=============================

Nginx auto renew ssl certificates script is developed to automate the certificate issue/renewal process.
auto renewal script is very important where nginx is configured as Gateway/Reverse Proxy. 
We have setup nginx as Gateway for all our application hosted for the demonstration. 

# Prerequisites

* Let's Encrypt Client(https://letsencrypt.org/getting-started/)

# Installation 

* Copy renew-ssl.sh and put it into let's encrypt checkout directory

# Configuration

* Change global variable as per your need.
* NGINX_CONF : Nginx configuration directory file
* LETSENC_BIN : path to let's encrypt binary 
* DEFAULT_DOMAIN : default domain name
* CERT_ISSUED_DOMAIN_LIST : Let's encrypt issued domain directories
* ISSUED_CERT_PATH : Let's encrypt path for issued certificates
* ISSUED_RENEWAL_CONF : Let's encrypt renewal configuration files
* iSSUED_CERT_ARCHIVE : Let's encrypt archive certificate files path

## Sample sub domain nginx configuration

Sample sub domain nginx configuration file for the support.silentinfotech.com

```
server {
	server_name support.silentinfotech.com;

	location / {
		return 301 https://$server_name$request_uri;
	}
}
# HTTPS server

server {
       listen          443 ssl;
       server_name support.silentinfotech.com;

       location / {
		proxy_pass http://support.silentinfotech.com;
       }
}


```

## Sample ssl.conf 

Create ssl.conf in /etc/nginx/conf.d folder.

```
ssl_session_cache    shared:SSL:10m;
ssl_session_timeout  10m;

# Perfect Forward Security
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH+aRSA+RC4 EECDH EDH+aRSA RC4 !aNULL !eNULL !LOW !3DES !MD5 !EXP !PSK !SRP !DSS +RC4 RC4";
ssl_certificate   /etc/letsencrypt/live/silentinfotech.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/silentinfotech.com/privkey.pem;
ssl_session_cache    shared:SSL:10m;
ssl_session_timeout  10m;

# Perfect Forward Security
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH+aRSA+RC4 EECDH EDH+aRSA RC4 !aNULL !eNULL !LOW !3DES !MD5 !EXP !PSK !SRP !DSS +RC4 RC4";
ssl_certificate   /etc/letsencrypt/live/silentinfotech.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/silentinfotech.com/privkey.pem;


```

replace silentinfotech with your default domain name

# Testing
 For simplicity we will use separate nginx configuration file for each sub-domain. we will use convention of adding "ssl-" prefix for 
 the domains. Script will auto detect the configuration files which starts with ssl- and 
 get list of sub-domains and put them into subject alternative field. Finally certificate with all sub domains as subject alternative will store at */etc/letsencrypt/live/$DEFAULT_DOMAIN/*

 ```
./renew-ssl.sh /etc/nginx/sites-enabled

```
