# Oracle Database on Oracle Linux.notes

组件名称：Oracle Database 
安装文档：https://docs.oracle.com/en/database/oracle/oracle-database/18/xeinl/procedure-installing-oracle-database-xe.html
配置文档：https://docs.oracle.com/en/database/oracle/oracle-database/18/xeinl/setting-oracle-database-xe-environment-variables.html
支持平台： Debian家族 | RHEL家族 | Windows |Oracle Linux|Docker

责任人：helin

## 概要

- Oracle Database，又名Oracle RDBMS，或简称Oracle。是[甲骨文公司](https://baike.baidu.com/item/甲骨文公司/430115)的一款[关系数据库管理系统](https://baike.baidu.com/item/关系数据库管理系统/11032386)。它是在数据库领域一直处于领先地位的产品。可以说Oracle数据库系统是目前世界上流行的关系数据库管理系统，系统可移植性好、使用方便、功能强，适用于各类大、中、小、微机环境。它是一种高效率、可靠性好的、适应高吞吐量的数据库方案。

## 环境要求

- 程序语言：Java
- 应用服务器：自带
- 数据库：oracle
- 依赖组件：

## 安装说明

在尝试安装Oracle数据库XE 18c之前，请从目标系统中卸载所有现有的具有SID的Oracle数据库XE或数据库。

在.NET下，Oracle数据库XE安装将占用大约9 GB的磁盘空间`/opt`。如果此磁盘分区没有所需的可用磁盘空间，则必须添加空间或将备用分区安装为`/opt/oracle`。该磁盘分区是软件和数据库将驻留的已定义Oracle Base。


#On Red Hat Enterprise Linux


# curl -o oracle-database-preinstall-18c-1.0-1.el7.x86_64.rpm https://yum.oracle.com/repo/OracleLinux/OL7/latest/x86_64/getPackage/oracle-database-preinstall-18c-1.0-1.el7.x86_64.rpm
# yum -y localinstall oracle-database-preinstall-18c-1.0-1.el7.x86_64.rpm
# wget https://download.oracle.com/otn-pub/otn_software/db-express/oracle-database-xe-18c-1.0-1.x86_64.rpm                                   
# yum -y localinstall oracle-database-xe-18c-1.0-1.x86_64.rpm  

#On Oracle Linux  

# wget https://download.oracle.com/otn-pub/otn_software/db-express/oracle-database-xe-18c-1.0-1.x86_64.rpm                                   
# yum -y localinstall oracle-database-xe-18c-1.0-1.x86_64.rpm  

## 路径

- 程序路径：/opt/oracle/product/18c/dbhomeXE
- 配置文件路径：/etc/sysconfig/oracle-xe-18c.conf
- 日志路径：/opt/oracle/cfgtoollogs/dbca/XE 
- 启动路径：/etc/init.d/oracle-xe-18c
- 其他...

## 配置

安装完成后，需要依次完成如下配置       

```
#创建Oracle XE数据库
sudo -s
(echo "password"; echo "password";) | /etc/init.d/oracle-xe-18c configure >> /xe_logs/XEsilentinstall.log 2>&1 #运行服务配置脚本  password为您设置的指定密码，适用于SYS，SYSTEM以及PDBADMIN管理用户帐户。Oracle建议输入的密码长度至少为8个字符，至少包含1个大写字母，1个小写字母和1个数字[0-9]。

#vi /etc/hosts
#服务器IP localhost    #在/etc/hosts 文件中增加一行 
vi /opt/oracle/product/18c/dbhomeXE/network/admin/tnsnames.ora
(ADDRESS = (PROTOCOL = TCP)(HOST = 服务器IP)(PORT = 1521)) #在tnsnames.ora文件中修改localhost为服务器IP

#开放远程访问


su - oracle ，然后直接在输入 ： vi .bash_profile



export ORACLE_BASE=/opt/oracle 
export ORACLE_HOME=/opt/oracle/product/18c/dbhomeXE
export ORACLE_SID=XE
export PATH=$PATH:$ORACLE_HOME/bin
source .bash_profile

systemctl start oracle-xe-18c




su oracle
sqlplus system 
输入密码：SYSTEM_password 
SQL> EXEC DBMS_XDB.SETLISTENERLOCALACCESS（FALSE）;

systemctl restart oracle-xe-18c

```







## 账号密码

后台账号

- 登录地址: https://服务器IP:5500/em
- 账号:sys或者system
- 密码：安装时设置的密码

## 服务

本项目安装后会自动生成

```
systemctl daemon-reload
systemctl enable oracle-xe-18c
```

## 环境变量  



```bash

```

### 版本号

通过如下的命令获取主要组件的版本号:

```
su oracle
sqlplus
```





## 常见问题

#### 有没有管理控制台？

*http:// 公网 IP:5500/em即可访问控制台，



#### 本项目需要开启哪些端口？

| item         | port |
| ------------ | ---- |
| 管理平台端口 | 5500 |
| 通讯端口     | 1521 |
|              |      |





## 日志

- 2020-05-27完成安装研究









