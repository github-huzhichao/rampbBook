# ASP.Net Core 运行在Linux(Ubuntu)

这段时间一直在研究asp.net core部署到linux，今天终于成功了，这里分享一下我的部署过程。

Linux Disibutaion：Ubuntu 14.04

Web Server:nginx、Kestrel

1、安装.net core

```
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
 
sudo apt-get update
 
sudo apt-get install dotnet-dev-1.0.0-preview2.1-003177
```

2、安装.net core成功之后，新建asp.net core mvc项目

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

3、到这一步还无法发布项目，我们需要安装npm,gulp,bower这三个工具

```
#安装npm
sudo apt-get install npm
 
#安装gulp
sudo npm install gulp -g
 
#安装bower
sudo npm install bower -g

```

4、完成之后，就可以对项目进行发布了

```
#发布项目，默认发布路径在当前项目下bin/Debug/netcoreapp1.1/publish/
dotnet publish

```

5、下面我们就要安装nginx做反向代理

```
#安装nginx
sudo apt-get install nginx
#启动nginx
sudo service nginx start
```

因为要使用nginx做asp.net core网站的反向代理，我们需要修改nginx的默认配置文件/etc/nginx/sites-available/default ，将以下内容替换默认配置：

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

保存退出

```
#检查nginx配置是否zhengque
sudo nginx -t
#重新加载nginx配置
sudo nginx -s reload
```

6、安装supervisor

我们部署的网站并不会自己启动并运行，这里我们就需要用到supervisor这个工具，保证网站的启动和持续运行。

```
#安装supervisor
sudo apt-get install supervisor
```

配置supervisor，进入目录（/etc/supervisor/conf.d/），新建配置文件hwapp.conf，将如下内容复制到文件中

```
[program:hwapp]
command=/usr/bin/dotnet /var/hwapp/publish/hwapp.dll --server.urls:http://*:5000
directory=/var/hwapp/publish
autostart=true
autorestart=true
stderr_logfile=/var/log/hwapp.err.log
stdout_logfile=/var/log/hwapp.out.log
environment=ASPNETCORE_ENVIRONMENT=Production
user=www-data
stopsignal=INT
```

关闭并启动supervisor

```
sudo service supervisor stop
sudo service supervisor start
```

到此，整个部署流程完成了！

下图在我部署在linux中的asp.net core网站的访问页面：

![访问页面](https://images2015.cnblogs.com/blog/446435/201612/446435-20161201155556881-798298854.png)