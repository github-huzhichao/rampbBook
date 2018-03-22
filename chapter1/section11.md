# 设置Azure WebSite黑白名单

Azure WebSite服务默认是不提供黑白名单，也就是说任何Internet用户都可以访问Azure WebSite，那么我们如何来给我们的网站设置黑白名单？

这里有一种方式，可以通过配置网站的配置文件（Web.config）来设置访问的黑白名单。

1、通过VS新建一个ASP.Net MVC项目

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fplg87j0z5j311y0k943n.jpg)
上图我已经打开网站的配置文件（Web.config）

2、给Azure WebSite网站设置白名单

首先百度查看本机的公网IP地址

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fplg9qlbxxj30iy05zjro.jpg)

通过Web.config文件中system.webServer节点下来设置访问网站的白名单，将本地公网IP加入白名单。

```
<system.webServer>
    <!--设置白名单-->
    <security>
      <ipSecurity allowUnlisted="false" denyAction="NotFound">
        <add allowed="true" ipAddress="101.231.141.202"/>
      </ipSecurity>
    </security>
</system.webServer>
```

编译之后，部署到Azure WebSite中，本机访问

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fplgavaflpj311y0jgju1.jpg)

使用一台公网IP为139.219.198.246的机器访问

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fplgbdifj1j311d0ivq8p.jpg)

3、给Azure WebSite网站设置黑名单

上面我们已经查过本机公网IP为：101.231.141.202

通过Web.config文件中system.webServer节点下来设置访问网站的黑名单，将本地公网IP加入黑名单。

```
<system.webServer>
    <!--设置黑名单-->
    <security>
      <ipSecurity allowUnlisted="true" denyAction="Forbidden">
        <add allowed="false" ipAddress="101.231.141.202"/>
      </ipSecurity>
    </security>
</system.webServer>
```

编译之后，部署到Azure WebSite中，本机访问

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fplgbrng1xj30re0jgt9x.jpg)

使用一台公网IP为139.219.198.246的机器访问

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fplgc4raoej311k0ixq8s.jpg)