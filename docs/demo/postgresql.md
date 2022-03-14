# PostgreSQL 安装和配置教程

本文是安装和配置 PostgreSQL 服务器的典型步骤，不能囊括所有安装环境和特殊要求，适合希望快速安装和试用使用 biReader 连接 PostgreSQL 数据库的用户。更详细和完整的技术文档请参考 PostgreSQL 官方文档。

## 安装

使用Windows 操作系统，先 [下载 PostgreSQL 安装程序](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)，按安装向导安装。

使用Linux 操作系统， 建议使用发行版自带的应用商店或包管理器进行安装，或直接使用命令进行安装，比如用 apt-get 进行安装，需要安装```postgresql```（服务器程序必备）和```postgresql-client```两个软件包（如果这台电脑需要使用客户端程序如 biReader的话）：

```
sudo apt-get install postgresql postgresql-client
```

安装后，有的安装程序会自动启动服务。如果没有自动启动，先 [启动服务](#start) 。服务启动成功后，用 psql 设置一下 postgres 账号的密码：

```

# sudo -i -u postgres
# psql
psql (9.6.10)
输入 "help" 来获取帮助信息.

postgres=# \password postgres;
输入新的密码：
再次输入：

```

如果要退出 psql， 输入 ```\q``` 后按回车。

<h2 id=start>启动服务</h2>

1. 如果是Windows操作系统，用以下命令启动服务（示例中 PostgreSQL 安装目录是 F:\PostgreSQL\11\，按自己的安装目录修改一下）：

``` Dos命令

C:\Users\Administrator>f:

F:\PostgreSQL\11\bin>cd f:\PostgreSQL\11\bin

F:\PostgreSQL\11\bin>pg_ctl -D f:\PostgreSQL\11\data start

```

修改配置后，如果需要重启服务，就用命令

``` Dos命令

F:\PostgreSQL\11\bin>pg_ctl -D f:\PostgreSQL\11\data restart

```

2. 如果是 Linux 操作系统，用命令：

```

#命令中的 '12 main' 用自己安装的版本和名称代替。
sudo -i -u postgres

#启动
pg_ctlcluster 12 main start 

#重启
pg_ctlcluster 12 main restart

#停止
pg_ctlcluster 12 main stop

```

有的安装实例没有pg_ctlcluster命令，可以用以下方式：

```
sudo /etc/init.d/postgresql start   # 开启
sudo /etc/init.d/postgresql stop    # 关闭
sudo /etc/init.d/postgresql restart # 重启
```

如果启动时报错```致命错误:  私钥文件"/etc/ssl/private/ssl-cert-snakeoil.key"具有由所在组或全局范围访问的权限```，先执行命令```sudo chmod 640 /etc/ssl/private/ssl-cert-snakeoil.key```，再试。

<h2 id=share>配置局域网访问</h2>

如果希望用局域网内的其它客户端台式机连接 PostgreSQL 服务器，需要在服务器端做一些设置。

1. 修改 pg_hba.conf 文件

如果服务器是Windows 操作系统，假设安装目录是 F:\PostgreSQL\11\，一般对应文件是 F:\PostgreSQL\11\data\pg_hba.conf。 如果服务器是Linux操作系统，文件目录可能是 /etc/postgresql/12/main/pg_hba.conf 。文件最后加上一行：
```
host    all     all             192.168.0.0/0            password
```
可以将“192.168.0.0”按需要修改自己局域网的掩码，也可以用“0.0.0.0”这样的方式。

2. Linux操作系统还需要开放侦听使用的IP地址。需要修改 postgresql.conf 文件。可以在服务器上搜索这个文件（一般是在安装目录下，比如 /etc/postgresql/12/main/postgresql.conf）。
用命令```sudo vim /etc/postgresql/12/main/postgresql.conf```在文件中找到```#listen_addresses = 'localhost'```，去掉前面的注释（如果有的话），将之改成```listen_addresses = '*'```

3. 以上两步修改完后，重启一下 postgresql 服务

## 创建数据库

用 postgres 账号（或其它有权限的账号）运行 psql ，用SQL语句```create database 数据库名```来创建数据库，比如：

Linux 下：

``` shell

# sudo -i -u postgres
# psql
postgres=# create database cashdemo;

```

Windows 下：

``` Dos

f:
cd PostgreSQL\11\bin
psql -h 127.0.0.1 -U postgres
postgres=# create database cashdemo;

```

## 备份数据库

用 pg_dump 命令进行备份。必须以对要备份的数据库具有读取权限的用户身份运行此命令。

比如下例将数据库 cashdemo 备份到文件  cashdemo.bak，Linux 下注意目录读写权限。

Linux 下：

```

# sudo -i -u postgres
# pg_dump cashdemo > cashdemo.bak

```

Windows 下：

``` Dos

f:
cd PostgreSQL\11\bin
pg_dump -U postgres cashdemo > cashdemo.bak

```

## 还原数据库

在备份文件所在目录下用 psql 程序还原数据库，比如下例用备份文件  cashdemo_pg.bak 还原 cashdemo 数据库：

Linux 下：

``` Shell

# sudo -i -u postgres
# psql cashdemo < cashdemo_pg.bak

```

Windows 下：

``` Dos

f:
cd PostgreSQL\11\bin
psql -U postgres cashdemo < cashdemo_pg.bak

```
