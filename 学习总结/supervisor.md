
1、安装supervisor

pip install supervisor

2、创建supervisor.conf

在/etc文件夹中创建supervisor文件夹

mkdir /etc/supervisor

通过echo_supervisord_conf程序生成supervisor的初始化配置文件

echo_supervisord_conf > /etc/supervisor/supervisord.conf

3、配置管理进程

1)创建conf.d文件保存要管理的进程配置参数
mkdir -p /etc/supervisor/conf.d

2)修改/etc/supervisor/supervisord.conf中的include参数，将/etc/supervisor/conf.d目录添加到include中

[include]
files = /etc/supervisor/config.d/*.ini

注：分号（;）开头的配置表示注释


4、启动、关闭supervisor服务

supervisord -c /etc/supervisor/supervisord.conf

killall supervisord

### bash终端

supervisorctl status
supervisorctl stop tomcat
supervisorctl start tomcat
supervisorctl restart tomcat
supervisorctl reread
supervisorctl update

### web管理界面
+ 开启管理界面

将以下参数前的分号(;)去掉

[inet_http_server]         ; inet (TCP) server disabled by default

port=0.0.0.0:9001          ; (ip_address:port specifier, *:port for all iface)

username=user              ; (default is no username (open server))

password=123               ; (default is no password (open server))

port：绑定访问IP和端口，这里是绑定的是本地IP和9001端口

username：登录管理后台的用户名
password：登录管理后台的密码

+ tomcat进程例子:

[program:tomcat]

command=/opt/apache-tomcat-8.0.35/bin/catalina.sh run

stdout_logfile=/opt/apache-tomcat-8.0.35/logs/catalina.out

autostart=true

autorestart=true

startsecs=5

priority=1

stopasgroup=true

killasgroup=true

> program:服务名称

> command:执行的命令

> stdout_logfile:log日志
