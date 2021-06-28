# 服务启停

使用由Websoft9提供的 Oracle Database 部署方案，可能需要用到的服务如下：

### Oracle Database

```shell
sudo systemctl start oracle-server
sudo systemctl stop oracle-server
sudo systemctl restart oracle-server
sudo systemctl status oracle-server

# you can use this debug mode if Oracle Database service can't run
oracle-server console
```

### MySQL

```shell
sudo systemctl start mysql
sudo systemctl stop mysql
sudo systemctl restart mysql
sudo systemctl status mysql
```

### Redis

```shell
systemctl start redis
systemctl stop redis
systemctl restart redis
systemctl status redis
```
