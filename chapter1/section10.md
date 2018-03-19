# ASP.Net Core 运行在Linux(CentOS)

Linux Disibutaion：CentOS 7.1

Web Server:Apache、Kestrel

1、安装.net core

```
sudo yum install libunwind libicu
curl -sSL -o dotnet.tar.gz https://go.microsoft.com/fwlink/?LinkID=835019
sudo mkdir -p /opt/dotnet && sudo tar zxf dotnet.tar.gz -C /opt/dotnet
sudo ln -s /opt/dotnet/dotnet /usr/local/bin
```

2、新建asp.net core mvc项目

```
#新建文件夹hwapp
mkdir hwapp
  
#进入hwapp文件夹
cd hwapp
  
#新建asp.net core mvc项目
dotnet new -t web
  
#还原.net core nuget包
dotnet restore
```

3、安装npm,gulp,bower

```
wget https://nodejs.org/dist/v6.9.1/node-v6.9.1-linux-x64.tar.xz
xz -d node-v6.9.1-linux-x64.tar.xz
#安装npm
sudo tar --strip-components 1 -xvf node-v* -C /usr/local
#安装gulp
sudo npm install gulp -g
#安装bower
sudo npm install bower -g
```

4、发布项目

```
cd /home/$1/hwapp
#发布项目
dotnet publish -c Release
```

5、安装并配置Apache

```
#更新
sudo yum update -y
#安装apache
sudo yum -y install httpd mod_ssl

```

进入/etc/httpd/conf.d目录，添加hwapp.conf文件，并将如下内容写入文件中

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hwapp-error.log
    CustomLog /var/log/httpd/hwapp-access.log common
</VirtualHost>
```

检查配置文件是否正确，如果看到"Syntax [OK]"则说明配置正确

```
sudo service httpd configtest
重启Apache

sudo systemctl restart httpd
sudo systemctl enable httpd
```

按理来说，到这一步应该已经可以访问我们部署的网站了，但是在CentOS下有一个细节需要注意：SELinux的安全策略导致网站无法访问。


```
#查看selinux状态
 
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
getenforce
#这里我们需要把SELinux的值修改为disabled
vi /etc/selinux/config
```

修改完SELinux，重启虚机

6、安装Supervisor

```
yum install python-setuptools
easy_install supervisor
配置Supervisor
```
```
mkdir /etc/supervisor
echo_supervisord_conf > /etc/supervisor/supervisord.conf
#指定配置文件
supervisord -c /etc/supervisor/supervisord.conf
指定守护的程序配置
```

```
vi /etc/supervisor/supervisord.conf
在supervisord.conf最后加入
```

[include]
files=conf.d/*.conf
进入目录/usr/lib/systemd/system/，新建一个“supervisord.service”文件


```
# dservice for systemd (CentOS 7.0+)
# by ET-CS (https://github.com/ET-CS)
[Unit]
Description=Supervisor daemon
 
[Service]
Type=forking
ExecStart=/usr/bin/supervisord -c /etc/supervisor/supervisord.conf
ExecStop=/usr/bin/supervisorctl shutdown
ExecReload=/usr/bin/supervisorctl reload
KillMode=process
Restart=on-failure
RestartSec=42s
 
[Install]
WantedBy=multi-user.target
```
执行命令
```
systemctl enable supervisord
systemctl start supervisord
```

配置守护进程，进入目录/etc/supervisor/conf.d/，新建文件“hwapp.conf”

```
[program:hwapp]
command=/usr/local/bin/dotnet /var/hwapp/hwapp.dll --server.urls:http://*:5000
directory=/var/hwapp/
autostart=true
autorestart=true
stderr_logfile=/var/log/hwapp.err.log
stdout_logfile=/var/log/hwapp.out.log
environment=ASPNETCORE_ENVIRONMENT=Production
user=azureuser
stopsignal=INT
```

重新加载配置

```
sudo supervisorctl reload
sudo supervisord
sudo supervisorctl
查看是否被守护进程拉起
```

```
ps -ef|grep dotnet
```