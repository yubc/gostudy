1. 获取mysql

wget https://cdn.mysql.com/archives/mysql-5.7/mysql-5.7.18-linux-glibc2.5-x86_64.tar

2. 解压

tar -xvf mysql-5.7.18-linux-glibc2.5-x86_64.tar

为了操作方便更改文件名
mv mysql-5.7.18-linux-glibc2.5-x86_64 mysql

3. 安装mysql

3.1 cd mysql

创建数据仓库目录
mkdir -p data/mysql

3.2 创建mysql用户名、组及目录

创建组
groupadd mysql

创建用户名
useradd -r -s /sbin/nologin -g mysql mysql -d /usr/local/mysql

3.3 改变目录所有者

chown -R mysql /usr/local/mysql
(后面的点表示所在目录)

3.4 配置参数设置

bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data/mysql

此处需要注意记录生成的临时密码，如下面的o5TueityhZ,N,临时密码在登录时需要使用

2017-08-19T07:43:32.395859Z 1 [Note] A temporary password is generated for root@localhost: o5TueityhZ,N

执行 bin/mysql_ssl_rsa_setup  --datadir=/usr/local/mysql/data/mysql

3.5 修改配置文件

cp support-files/mysql.server /etc/init.d/mysqld

vi /etc/init.d/mysqld

#需修改的参数

basedir=/usr/local/mysql

datadir=/usr/local/mysql/data/mysql

修改 /etc/my.cnf 通过scp my.cnf root@192.168.235.157:/etc/my.cnf把本地文件上传到服务器

3.6 启动mysql

重新对目录所有者设置
chown -R mysql /usr/local/mysql

执行 bin/mysqld_safe --user=mysql &

通过 ps -ef | grep mysqld命令查看mysql服务是否启动

4. 登录mysql

bin/mysql -u root -p 输入临时密码

重新设置密码

mysql> set password=password('root');

添加用户

mysql> grant all privileges on *.* to 'root'@'%' identified by 'root';
mysql> flush privileges;

5. 添加系统路径和自动启动mysql

vi ~/.bash_profile

在bin后添加mysqld路径
PATH=$PATH:$HOME/bin:/usr/local/mysql/bin

保存,刷新 source ~/.bash_profile

设置自启动

chkconfig --add mysqld

chkconfig --level 345 mysqld on

6. 主从设置

6.1 主服务设置

* 修改配置文件my.cnf
#编号

server-id=1

#允许mysql使用binlog，同时为主从复制打开了大门

log-bin=mysql-bin

#保存时间(默认10天)

expire_logs_days=5

#需要复制的数据库

#binlog-do-db=数据库名3

#忽略复制的数据库

binlog-ignore-db=mysql
binlog-ignore-db=information_schema

* 重启mysql服务

mysql> service mysqld restart
* 设置复制用户名和密码

mysql> grant all privileges on *.* to 'slave'@'%' identified by '123456';

* 查看主服务器状态

msyql> show master status;

| File| Position | Binlog_Do_DB | Binlog_Ignore_DB| Executed_Gtid_Set |
|-----|----------|--------------|-----------------|-------------------|
| mysql-bin.000009 |  830 || mysql,information_schema |               |


6.2 从服务器设置

* 修改配置文件my.cnf

server-id=2

relay-log-index=slave-relay-bin.index #(中继日志的索引文件）

relay-log=slave-relay-bin  #（中继日志的文件前缀）

mysql> stop slave;

mysql> change master to master_host='192.168.235.148',master_user='slave',master_password='123456',master_log_file='mysql-bin.000009', master_log_pos=6503;

master_host为主服务器ip master_user为主服务器用户名 master_password为主服务器登录密码  master_log_file为主服务器二进制文件 master_log_pos为主服务器起始位置(二进制文件和起始位置通过在主服务器中show master status命令查看)

mysql> start slave;

mysql> show slave status\G;

下列参数都为yes表示主从成功

Slave_IO_Running: Yes

Slave_SQL_Running: Yes

> 问题解决

Slave_IO_Running: Yes

Slave_SQL_Running: No

1. 重新对slave进行手动同步
2. mysql> stop slave;

mysql> set GLOBAL SQL_SLAVE_SKIP_COUNTER=1;

mysql> start slave;