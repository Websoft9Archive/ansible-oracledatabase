---
sidebarDepth: 3
---

# Oracle Database速学

## 概念

### 数据库

Oracle Database同其他数据一样，本质上就数据的物理存储。包括数据文件ORA、DBF、控制文件、联机日志、参数文件。

#### 物理结构

数据库的物理存储结构是由一些多种物理文件组成，主要有数据文件、控制文件、重做日志文件、归档日志文件、参数文件、口令文件、警告文件等。

 - 控制文件：存储实例、数据文件及日志文件等信息的二进制文件。
 - 数据文件：存储数据，以.dbf做后缀。一句话：一个表空间对多个数据文件，一个数据文件只对一个表空间。
 - 日志文件：即Redo Log Files和Archivelog Files。记录数据库修改信息。
 - 参数文件：记录基本参数。spfile和pfile。
 - 警告文件：show parameter background_dump_dest---使用共享服务器连接。
 - 跟踪文件：show parameter user_dump_dest---使用专用服务器连接。

#### 逻辑结构

数据库逻辑结构由逻辑存储结构(表空间,段,范围,块)和逻辑数据结构(表、视图、序列、存储过程、同义词、索引、簇和数据库链等)组成,而其中的模式对象(逻辑数据结构)和关系形成了数据库的关系设计。逻辑存储结构包括表空间、段和范围，用于描述怎样使用数据库的物理空间。

 - 表空间：表空间是一个用来管理数据存储逻辑概念，表空间只是和数据文件（ORA或者DBF文件）发生关系，数据文件是物理的，一个表空间可以包含多个数据文件，而一个数据文件只能隶属一个表空间。
 - 段（Segment）：是表空间中一个指定类型的逻辑存储结构，它由一个或多个范围组成，段将占用并增长存储空间。其中包括：数据段：用来存放表数据；索引段：用来存放表索引；临时段：用来存放中间结果；
回滚段：用于出现异常时，恢复事务。
 - 范围（Extent）：是数据库存储空间分配的逻辑单位，一个范围由许多连续的数据块组成，范围是由段依次分配的，分配的第一个范围称为初始范围，以后分配的范围称为增量范围。
 - 数据块（Block）：是数据库进行IO操作的最小单位，它与操作系统的块不是一个概念。oracle数据库不是以操作系统的块为单位来请求数据，而是以多个Oracle数据库块为单位。

#### 数据库名

数据库名就是一个数据库的标识，就像人的身份证号一样。他用参数DB_NAME表示，如果一台机器上装了多个数据库，那么每一个数据库都有一个数据库名。在创建数据库时就应考虑好数据库名，
并且在创建完数据库之后，数据库名不宜修改，即使要修改也会很麻烦。因为，数据库名还被写入控制文件中，控制文件是以 二进制型式存储的，用户无法修改控制文件的内容。假设用户修改了
参数文件中的数据库名，即修改DB_NAME的值。


### 数据库实例

数据库实例（instance）是一组用于管理数据库文件的内存结构。由一系列的后台进程（Backguound Processes)和内存结构（Memory Structures)组成。一个数据库可以有多个实例。

#### 实例结构

当一个实例启动时，Oracle 数据库分配一个称为系统全局区（SGA）的内存区域，并启动一个或多个后台进程。
SGA 的作用包括：
 - 维护多个进程和线程并发访问的内部数据结构
 - 缓存从磁盘读取的数据块
 - 在写入在线重做日志文件之前缓冲重做数据
 - 存储 SQL 执行计划
同一个服务器上的 Oracle 进程之间共享 SGA。Oracle 进程与 SGA 的交互方式取决于操作系统。

一个数据库实例包括多个后台进程（background process）。服务器进程（server process），以及分配给它们的内存，也位于实例之中。实例在服务器进程结束后仍然继续存在。
下图显示了 Oracle Database 实例中的主要组件。
[](https://libs.websoft9.com/Websoft9/DocsPicture/zh/oracle_database/oracle-instancecomponent-websoft9.png)

#### 实例名和实例配置

数据库实例名是用于和操作系统进行联系的标识，就是说数据库和操作系统之间的交互用的是数据库实例名。实例名也被写入参数文件中，该参数为instance_name。
数据库名和实例名可以相同也可以不同。在单实例情况下，数据库名和实例名是一对一的关系，但如果在应用集群（Oracle RAC）配置中，数据库名和实例名是一对多的关系。无论是单实例还是 Oracle RAC 配置，一个实例每次只能与一个数据库关联。管理员可以启动一个实例，然后加载（关联）一个数据库，但是不能同时加载两个数据库。
下图显示了两种可能的数据库实例配置。



## 监听器(LISTENER)

监听器是Oracle Database基于服务器端的一种网络服务，主要用于监听客户端向数据库服务器端提出的连接请求。既然是基于服务器端的服务，那么它也只存在于数据库服务器端，进行监听器的设置也是在数据库服务器端完成的。

### SQL vs Oracle Database

The basic concepts in mongodb are document, collection, and databases. let's take SQL as an example to help you better understand Oracle Database.

| SQL Term/concept | Oracle Database Term/concept | explain |
| :--- | :--- | :--- |
| database | database | Database Instance |
| table | collection | databae table/collection |
| row | document | table row/document |
| column | field | Data field/domain |
| index | index | index |
| table joins |   | Oracle Database no this |
| primary key | primary key | keyPrimary key, Oracle Database automatically sets the _id field as the primary key |

Through the example below, we can also understand some concepts in Mongo more intuitively:

![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/mongodb/nosqlvssql-websoft9.png)


### Oracle Database 数据类型

Oracle Database 的数据类型非常类似 JavaScript 对象。

**字符串** - 这是用于存储数据的最常用的数据类型。Oracle Database中的字符串必须为`UTF-8`。

**整型** - 此类型用于存储数值。 整数可以是`32`位或`64`位，具体取决于服务器。

**布尔类型** - 此类型用于存储布尔值(`true` / `false`)值。

**双精度浮点数** - 此类型用于存储浮点值。

**最小/最大键** - 此类型用于将值与最小和最大`BSON`元素进行比较。

**数组** - 此类型用于将数组或列表或多个值存储到一个键中。

**时间戳** - `ctimestamp`，当文档被修改或添加时，可以方便地进行录制。

**对象** - 此数据类型用于嵌入式文档。

**对象** - 此数据类型用于嵌入式文档。

**Null** - 此类型用于存储`Null`值。

**符号** - 该数据类型与字符串相同; 但是，通常保留用于使用特定符号类型的语言。

**日期** - 此数据类型用于以UNIX时间格式存储当前日期或时间。您可以通过创建日期对象并将日，月，年的日期进行指定自己需要的日期时间。

**对象ID** - 此数据类型用于存储文档的ID。

**二进制数据** - 此数据类型用于存储二进制数据。

**代码** - 此数据类型用于将JavaScript代码存储到文档中。

**正则表达式** - 此数据类型用于存储正则表达式。

### 规划数据模型

Oracle Database 作为一种数据库，与传统的 RDBMS 的使用方式也有相似之处，即规划数据模型，建立数据库范式。只有这种，才能更好的发挥数据库的性能。  

![](http://libs.websoft9.com/Websoft9/DocsPicture/en/mongodb/mongodb-datamodel-websoft9.png)


数据规划的主要设计要点包括：

* 使用数据范式
* 使用嵌入式文档反范式
* 使用固定集合
* 考虑文档增大
* 规划索引、分片和复制
* 规划数据生命周期

## 配置

### 启动

安装Oracle Database后，启动 bin 目录下的可执行文件 mongod 就可以启动 Oracle Database 服务，如果你配置了 Systemd，也可以通过 `systemctl start mongod` 以后台的形式启动 Oracle Database。  

Oracle Database 在启动的时候，可以通过命令接受一序列参数，也可以通过配置文件接受参数：  

**命令行参数**

```
  -v [ --verbose ] [=arg(=v)]           be more verbose (include multiple times
                                        for more verbosity e.g. -vvvvv)
  --quiet                               quieter output
  --port arg                            specify port number - 27017 by default
  --logpath arg                         log file to send write to instead of
                                        stdout - has to be a file, not
                                        directory
  --syslog                              log to system's syslog facility instead
                                        of file or stdout
  --syslogFacility arg                  syslog facility used for mongodb syslog
                                        message
  --logappend                           append to logpath instead of
                                        over-writing
  --logRotate arg                       set the log rotation behavior
                                        (rename|reopen)
  --timeStampFormat arg                 Desired format for timestamps in log
                                        messages. One of ctime, iso8601-utc or
                                        iso8601-local
  --setParameter arg                    Set a configurable parameter
  -h [ --help ]                         show this usage information
  --version                             show version information
  -f [ --config ] arg                   configuration file specifying
                                        additional options
  --bind_ip arg                         comma separated list of ip addresses to
                                        listen on - localhost by default
  --bind_ip_all                         bind to all ip addresses
  --ipv6                                enable IPv6 support (disabled by
                                        default)
  --listenBacklog arg (=128)            set socket listen backlog size
  --maxConns arg                        max number of simultaneous connections
                                        - 1000000 by default
  --pidfilepath arg                     full path to pidfile (if not set, no
                                        pidfile is created)
  --timeZoneInfo arg                    full path to time zone info directory,
                                        e.g. /usr/share/zoneinfo
  --keyFile arg                         private key for cluster authentication
  --noauth                              run without security
  --transitionToAuth                    For rolling access control upgrade.
                                        Attempt to authenticate over outgoing
                                        connections and proceed regardless of
                                        success. Accept incoming connections
                                        with or without authentication.
  --clusterAuthMode arg                 Authentication mode used for cluster
                                        authentication. Alternatives are
                                        (keyFile|sendKeyFile|sendX509|x509)
  --nounixsocket                        disable listening on unix sockets
  --unixSocketPrefix arg                alternative directory for UNIX domain
                                        sockets (defaults to /tmp)
  --filePermissions arg                 permissions to set on UNIX domain
                                        socket file - 0700 by default
  --fork                                fork server process
  --slowms arg (=100)                   value of slow for profile and console
                                        log
  --slowOpSampleRate arg (=1)           fraction of slow ops to include in the
                                        profile and console log
  --networkMessageCompressors [=arg(=disabled)] (=snappy)
                                        Comma-separated list of compressors to
                                        use for network messages
  --auth                                run with security
  --clusterIpSourceWhitelist arg        Network CIDR specification of permitted
                                        origin for `__system` access.
  --profile arg                         0=off 1=slow, 2=all
  --cpu                                 periodically show cpu and iowait
                                        utilization
  --sysinfo                             print some diagnostic system
                                        information
  --noIndexBuildRetry                   don't retry any index builds that were
                                        interrupted by shutdown
  --noscripting                         disable scripting engine
  --notablescan                         do not allow table scans
  --shutdown                            kill a running server (for init
                                        scripts)

Replication options:
  --oplogSize arg                       size to use (in MB) for replication op
                                        log. default is 5% of disk space (i.e.
                                        large is good)
  --master                              Master/slave replication no longer
                                        supported
  --slave                               Master/slave replication no longer
                                        supported

Replica set options:
  --replSet arg                         arg is <setname>[/<optionalseedhostlist
                                        >]
  --replIndexPrefetch arg               specify index prefetching behavior (if
                                        secondary) [none|_id_only|all]
  --enableMajorityReadConcern [=arg(=1)] (=1)
                                        enables majority readConcern

Sharding options:
  --configsvr                           declare this is a config db of a
                                        cluster; default port 27019; default
                                        dir /data/configdb
  --shardsvr                            declare this is a shard db of a
                                        cluster; default port 27018

SSL options:
  --sslOnNormalPorts                    use ssl on configured ports
  --sslMode arg                         set the SSL operation mode
                                        (disabled|allowSSL|preferSSL|requireSSL
                                        )
  --sslPEMKeyFile arg                   PEM file for ssl
  --sslPEMKeyPassword arg               PEM file password
  --sslClusterFile arg                  Key file for internal SSL
                                        authentication
  --sslClusterPassword arg              Internal authentication key file
                                        password
  --sslCAFile arg                       Certificate Authority file for SSL
  --sslClusterCAFile arg                CA used for verifying remotes during
                                        outbound connections
  --sslCRLFile arg                      Certificate Revocation List file for
                                        SSL
  --sslDisabledProtocols arg            Comma separated list of TLS protocols
                                        to disable [TLS1_0,TLS1_1,TLS1_2]
  --sslWeakCertificateValidation        allow client to connect without
                                        presenting a certificate
  --sslAllowConnectionsWithoutCertificates
                                        allow client to connect without
                                        presenting a certificate
  --sslAllowInvalidHostnames            Allow server certificates to provide
                                        non-matching hostnames
  --sslAllowInvalidCertificates         allow connections to servers with
                                        invalid certificates
  --sslFIPSMode                         activate FIPS 140-2 mode at startup

Storage options:
  --storageEngine arg                   what storage engine to use - defaults
                                        to wiredTiger if no data files present
  --dbpath arg                          directory for datafiles - defaults to
                                        /data/db
  --directoryperdb                      each database will be stored in a
                                        separate directory
  --noprealloc                          disable data file preallocation - will
                                        often hurt performance
  --nssize arg (=16)                    .ns file size (in MB) for new databases
  --quota                               limits each database to a certain
                                        number of files (8 default)
  --quotaFiles arg                      number of files allowed per db, implies
                                        --quota
  --smallfiles                          use a smaller default file size
  --syncdelay arg (=60)                 seconds between disk syncs (0=never,
                                        but not recommended)
  --upgrade                             upgrade db if needed
  --repair                              run repair on all dbs
  --repairpath arg                      root directory for repair files -
                                        defaults to dbpath
  --journal                             enable journaling
  --nojournal                           disable journaling (journaling is on by
                                        default for 64 bit)
  --journalOptions arg                  journal diagnostic options
  --journalCommitInterval arg           how often to group/batch commit (ms)

WiredTiger options:
  --wiredTigerCacheSizeGB arg           maximum amount of memory to allocate
                                        for cache; defaults to 1/2 of physical
                                        RAM
  --wiredTigerJournalCompressor arg (=snappy)
                                        use a compressor for log records
                                        [none|snappy|zlib]
  --wiredTigerDirectoryForIndexes       Put indexes and data in different
                                        directories
  --wiredTigerMaxCacheOverflowFileSizeGB arg (=0)
                                        Maximum amount of disk space to use for
                                        cache overflow; Defaults to 0
                                        (unbounded)
  --wiredTigerCollectionBlockCompressor arg (=snappy)
                                        block compression algorithm for
                                        collection data [none|snappy|zlib]
  --wiredTigerIndexPrefixCompression arg (=1)
                                        use prefix compression on row-store
                                        leaf pages

Free Monitoring options:
  --enableFreeMonitoring arg            Enable Cloud Free Monitoring
                                        (on|runtime|off)
  --freeMonitoringTag arg               Cloud Free Monitoring Tags

```

**配置文件参数**

配置文件所用的参数与命令行有一些差异，Oracle Database 当前采用配置组+配置段的方式组织[配置文件](https://docs.mongodb.com/v4.0/reference/configuration-options/#conf-file)，配置组主要包括：

* systemLog Options
* processManagement Options
* cloud Options
* net Options
* security Options
* setParameter Option
* storage Options
* operationProfiling Options
* replication Options
* sharding Options
* auditLog Options
* snmp Options 

下面是一个典型的配置文件内容：  

```
processManagement:
   fork: true
net:
   bindIp: localhost
   port: 27017
storage:
   dbPath: /var/lib/mongo
systemLog:
   destination: file
   path: "/var/log/mongodb/mongod.log"
   logAppend: true
storage:
   journal:
      enabled: true
```

### Oracle Database shell

Oracle Database shell 是一个可执行文件，位于安装路径的 bin 目录下，执行 `mongo` 命令即可启动 Oracle Database shell。它是基于 JavaScript 语法的，即可以使用 JavaScript 语法与数据库进行交付。

```
[root@mongodb-test-centos7 ~]# mongo
Oracle Database shell version v4.0.18
connecting to: mongodb://127.0.0.1:27017/?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("e808b886-30db-41dd-9464-40b52f041107") }
Oracle Database server version: 4.0.18
> help
        db.help()                    help on db methods
        db.mycoll.help()             help on collection methods
        sh.help()                    sharding helpers
        rs.help()                    replica set helpers
        help admin                   administrative help
        help connect                 connecting to a db help
        help keys                    key shortcuts
        help misc                    misc things to know
        help mr                      mapreduce

        show dbs                     show database names
        show collections             show collections in current database
        show users                   show users in current database
        show profile                 show most recent system.profile entries with time >= 1ms
        show logs                    show the accessible logger names
        show log [name]              prints out the last segment of log in memory, 'global' is default
        use <db_name>                set current database
        db.foo.find()                list objects in collection foo
        db.foo.find( { a : 1 } )     list objects in foo where a == 1
        it                           result of the last line evaluated; use to further iterate
        DBQuery.shellBatchSize = x   set default number of items to display on shell
        exit                         quit the mongo shell

```

Oracle Database shell 有两种方式与数据库进行交互：

* 命令行交互式操作
* 运行存放在文件中的命令脚本（例如：shell_script.js）


### 账户和访问控制

Oracle Database 支持账号访问控制方式，也支持 Kerberos, LDAP 等外部身份验证机制。本章我们只介绍账户方式相关的四个关键要点：

#### 数据库、用户和角色

通过阅读下面的代码理解用户、数据库和角色的关系
```
use reporting
db.createUser(
  {
    user: "reportsUser",
    pwd: "12345678",
    roles: [
       { role: "read", db: "reporting" },
       { role: "read", db: "products" },
       { role: "read", db: "sales" },
       { role: "readWrite", db: "accounts" }
    ]
  }
)
```
对Oracle Database来说，每个用户都存在一个数据库中（区别于MySQL中所有的用户存储在一个系统数据库中）  

系统默认，会自动创建 admin 数据库，这是一个特殊数据库，提供了普通数据库没有的功能，对于具备全局管理权限的数据库用户，必须存储在这个 admin 数据中。

#### 启用认证

创建了用户，并不会要求登录，只有开启了认证才会要求登录访问数据库。

打开Oracle Database配置文件：*/etc/mongod.conf*，将authorization字段改为 enabled 即启用认证。

```
security:
  authorization: disabled
```

为了方便试用，默认情况下认证已关闭。


## 使用

### 应用程序访问

Oracle Database支持主流的开发程序直接访问，包括：PHP、Java、Python、Node.js等

### 工具

Oracle Database 官方提供了更多的工具，包括：

**Oracle Database Atlas Open Service Broker**  
Learn how you can use the Atlas Open Service Broker to deploy Atlas clusters and manage database users from within Kubernetes.

**Oracle Database BI Connector**  
Reference guide for the Oracle Database BI Connector. Learn how you can use business intelligence tools and SQL to query data stored in Oracle Database.

**Oracle Database Charts**  
Reference guide for Oracle Database Charts. Learn how to create visualizations of Oracle Database data quickly and easily.

**Oracle Database Command Line Interface**  
Learn how to use the Oracle Database Command Line Interface to quickly interact with your Oracle Database deployments for easier testing and scripting.

**Oracle Database Compass**  
Reference guide for Oracle Database Compass. Learn to use Oracle Database Compass's graphical user interface to view and analyze data stored in Oracle Database.

**Oracle Database Database Tools**  
Tools for interfacing with a Oracle Database cluster, such as importing/exporting data.

**Oracle Database Kafka Connector**  
Learn how to persist data from Kafka topics as a data sink into Oracle Database as well as publish changes from Oracle Database into Kafka topics as a data source.

**Oracle Database Kubernetes Operator**  
Learn how you can use the Kubernetes Operator to run Oracle Database Enterprise on Kubernetes and configure Cloud or Ops Manager for backup and monitoring.

**Oracle Database Spark Connector**  
Reference guide for the Oracle Database Spark Connector. Learn how you can use Oracle Database with Apache Spark.