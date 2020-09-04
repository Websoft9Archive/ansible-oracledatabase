# Parameters

The RabbitMQ deployment package contains a sequence software (referred to as "components") required for RabbitMQ to run. The important information such as the component name, installation directory path, configuration file path, port, version, etc. are listed below.

## Path

### RabbitMQ

RabbitMQ installation directory:  */data/rabbitmq*  
RabbitMQ logs directory:  */data/logs/rabbitmq*  

### Nginx

Nginx vhost configuration file: */etc/nginx/conf.d/default.conf*    
Nginx main configuration file: */etc/nginx/nginx.conf*   
Nginx logs file: */var/log/nginx*  
Nginx rewrite rules directory: */etc/nginx/conf.d/rewrite* 

### MYSQL

MySQL installation directory: */usr/local/mysql*  
MySQL data directory: */data/mysql*  
MySQL configuration file: */etc/my.cnf*    
MySQL Web Management URL: *http://Internet IP/9panel*, get credential from [Username and Password](/stack-accounts.md)

## Ports

You can control(open or shut down) ports by **[Security Group Setting](https://support.websoft9.com/docs/faq/zh/tech-instance.html)** of your Cloud Server whether the port can be accessed from Internet.

You can run the cmd `netstat -tunlp` to list all used ports, and we list the following most useful ports for you:

| Name | Number | Use |  Necessity |
| --- | --- | --- | --- |
| HTTP | 8161 | HTTP requests for RabbitMQ Console| Required |
| HTTPS | 5672 | epmd | Optional |
| TCP | 55672 | Erlang distribution | Optional |


## Version

You can see the version from product page of Marketplace. However, after being deployed to your server, the components will be automatically updated, resulting in a certain change in the version number. Therefore, the exact version number should be viewed by running the command on the server:

```shell
# Check all components version
sudo cat /data/logs/install_version.txt

# Linux Version
lsb_release -a

# Nginx  Version
nginx -V

# Java version
java -v

# Docker Version
docker -v

# erlang  Version
yum info erlang
apt show erlang

# RabbitMQ version
rabbitmqctl status | grep RabbitMQ*
```
