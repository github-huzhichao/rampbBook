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

![新建一个控制台应用程序](https://images2015.cnblogs.com/blog/446435/201611/446435-20161103154924658-1368385571.png)

3、将代码文件上传到Linux上

这里我使用的是pscp command line工具上传文件到linux

![将代码文件上传到Linux上](https://images2015.cnblogs.com/blog/446435/201611/446435-20161103155152455-258563223.png)

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

![dotnet restore](https://images2015.cnblogs.com/blog/446435/201611/446435-20161103155656565-1374370142.png)

输入linux命令：dotnet run

![dotnet run](https://images2015.cnblogs.com/blog/446435/201611/446435-20161103155746893-979282090.png)
大功告成！！！.Net程序成功运行在Linux上了。