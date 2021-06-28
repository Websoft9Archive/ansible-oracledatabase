# Start or Stop the Services

These commands you must know when you using the Oracle Database of Websoft9

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
