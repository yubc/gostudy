
1. 创建3台虚拟机

管理节点(ndb_mgmd) 192.168.235.142

数据节点(ndbd) 192.168.235.143  192.168.235.144

SQL节点(mysqld) 192.168.235.143  192.168.235.144


2.  mysql cluster下载

[https://dev.mysql.com/downloads/cluster/](https://dev.mysql.com/downloads/cluster/)

或者通过wget https://cdn.mysql.com//Downloads/MySQL-Cluster-7.4/mysql-cluster-gpl-7.4.16-linux-glibc2.12-x86_64.tar.gz

3. 卸载mysql和安装准备

rpm -qa | grep SQL

rpm -e mysql...

删除/etc/my.cnf  /var/lib/mysql

关闭防火墙

systemctl stop firewalld.service

禁用防火墙

systemctl disable firewalld.service

查看防火墙的状态

systemctl status firewalld.service

查看本机ip

ip addr

关闭SELinux

vi /etc/selinux/config

设置SELINUX=disable

4. 安装管理节点(192.168.235.142)

解压文件

tar -xvf mysql-cluster-gpl-7.4.16-linux-glibc2.12-x86_64.tar.gz -C /usr/local

为了方便操作修改文件夹名
mv mysql-cluster-gpl-7.4.16-linux-glibc2.12-x86_64 mysqlc

创建安装目录

mkdir -p /usr/local/mysql/bin

mkdir -p /usr/local/mysql/ndbdata

mkdir -p /usr/local/mysql/config

拷贝执行文件

cd /usr/local/mysqlc

cp /bin/ndb_mgmd /usr/local/mysql/bin

cp /bin/ndb_mgm /usr/local/mysql/bin

添加执行文件路径

vi ~/.bash_profile

PATH=$PATH:$HOME/bin:/usr/local/mysql/bin

创建配置文件目录

mkdir -p /usr/local/mysql/mysql-cluster

在config文件夹中编辑配置文件,配置文件内容

```m
[ndb_mgmd default]
datadir=/usr/local/mysql/ndbdata

[ndbd default]
NoOfReplicas=2 #复制成员个数根据ndbd个数确定
DataMemory=80M #数据存储分配内容
IndexMemory=18M #索引存储分配内存
datadir=/usr/local/mysql/ndbdata

[ndb_mgmd]
NodeId=1
hostname=192.168.235.142

[ndbd]
NodeId=11
hostname=192.168.235.143

[ndbd]
NodeId=12
hostname=192.168.235.144

[mysqld]
NodeId=81
hostname=192.168.235.143

[mysqld]
NodeId=82
hostname=192.168.235.144

[mysqld]
NodeId=85
hostname=192.168.235.142
```

5. 安装数据节点(192.168.235.143 192.168.235.144)

解压文件

tar -xvf mysql-cluster-gpl-7.4.16-linux-glibc2.12-x86_64.tar.gz -C /usr/local

为了方便操作修改文件夹名
mv mysql-cluster-gpl-7.4.16-linux-glibc2.12-x86_64 mysql
(如果数据节点和sql节点不在同一在设备上，只需把bin下ndbd拷贝到指定文件夹)

配置执行文件路径

/usr/local/mysql/bin是执行文件的路径添加到path后(注意在路径前加冒号)

vi ~/.bash_profile

PATH=$PATH:$HOME/bin:/usr/local/mysql/bin

修改配置文件

vi /etc/my.cnf

```m
[mysql_cluster] #配置数据节点连接管理节点
ndb-connectstring=192.168.235.142
```

6. 安装sql节点

创建ndbdata文件夹

添加用户

groupadd msyql

useradd -g mysql -s /usr/sbin/nologin mysql

添加权限

chown  -R mysql:mysql /usr/local/mysql

拷贝配置文件(数据节点和sql节点在同一设备上可忽略)

cp /usr/local/mysql/support-files/my-large.cnf /etc/my.cnf

配置执行文件路径(数据节点和sql节点在同一设备上可忽略)

/usr/local/mysql/bin是执行文件的路径添加到path后(注意在路径前加冒号)

vi ~/.bash_profile

PATH=$PATH:$HOME/bin:/usr/local/mysql/bin

修改配置文件

vi /etc/my.cnf

```m
[mysqld]

basedir = /usr/local/mysql
datadir = /usr/local/mysql/ndbdata
socket=/tmp/mysql.sock

ndbcluster
ndb-connectstring = 192.168.235.142

```

初始化数据库

cd /usr/local/mysql

scripts/mysql_install_db --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data

 如果出现FATAL ERROR: please install the following Perl modules before executing ./scripts/mysql_install_db:Data::Dumper，执行:  
#安装 perl-module  
#yum install -y perl-Module-Install.noarch  

创建守护进程

cp /usr/local/mysql/support-files/mysql.server /etc/rc.d/init.d/mysqld

配置守护进程

chkconfig –-add mysqld

chkconfig –-level 35 mysqld on

7. 启动和关闭msyql 

ndb_mgmd -f /usr/local/mysql/config/config.ini --initial

ndbd –initial #(第一次启动必须添加选项，另外备份/恢复，修改配置文件也需要执行)

ndbd    #不是第一次启动需要执行的命令

service mysqld start

a、关闭SQL节点

/etc/rc.d/init.d/mysqld stop 或service mysqld stop

b、关闭管理节点

ndb_mgm〉 shutdown

注意建表语句后面一定要加上 engine=ndbcluster