# .Net程序跑在Linux上

.Net越来越拥抱开源了，今天就试了如何让.Net程序跑在Linux上，果然再无人可以阻挡.Net的脚步了。

Linux Disibutaion:Open Logic 7.2

1、Install .NET Core SDK

SSH进入Linux,输入如下命令：
```

sudo yum install libunwind libicu
curl -sSL -o dotnet.tar.gz https://go.microsoft.com/fwlink/?LinkID=827529
sudo mkdir -p /opt/dotnet && sudo tar zxf dotnet.tar.gz -C /opt/dotnet
sudo ln -s /opt/dotnet/dotnet /usr/local/bin

```

这里我们就安装好了.Net程序运行的环境。

2、打开VS，新建一个控制台应用程序

![](https://ws4.sinaimg.cn/large/006tNc79gy1fpj8p8f8ujj311q0k3q66.jpg)

3、将代码文件上传到Linux上

这里我使用的是pscp command line工具上传文件到linux

![](https://ws4.sinaimg.cn/large/006tNc79gy1fpj8ps0ghkj30r2071t93.jpg)

代码上传成功之后，我们的程序要跑在linux上，还缺少一个project.json的文件。

输入linux命令：vi project.json

进入vi编辑器加入如下内容：

```
{
  "version": "1.0.0-*",
  "buildOptions": {
    "debugType": "portable",
    "emitEntryPoint": true
  },
  "dependencies": {},
  "frameworks": {
    "netcoreapp1.0": {
      "dependencies": {
        "Microsoft.NETCore.App": {
          "type": "platform",
          "version": "1.0.1"
        }
      },
      "imports": "dnxcore50"
    }
  }
}
```

保存退出后。

输入linux命令：dotnet restore

![](https://ws3.sinaimg.cn/large/006tNc79gy1fpj8punn9qj30lv02bq2x.jpg)

输入linux命令：dotnet run

![](https://ws2.sinaimg.cn/large/006tNc79gy1fpj8tamq6hj30mx0jgjsg.jpg)大功告成！！！.Net程序成功运行在Linux上了。