# Log4Net详细配置

关于Log4Net配置主要分几步

第一步：下载log4net.dll（log4net官网：http://logging.apache.org/log4net/download_log4net.cgi）

第二步：在项目中添加对log4net.dll的引用

第三步：配置log4net

```
<?xml version="1.0" encoding="utf-8" ?>
<log4net>
<!--The config of System's default level - FATAL-->
<appender name="FATAL" type="log4net.Appender.RollingFileAppender">
<file value="Log/fatal"/>
<appendToFile value="true"/>
<rollingStyle value="Composite" />
<maxSizeRollBackups value="100" />
<maximumFileSize value="102400KB" />
<staticLogFileName value="false" />
<datePattern value="_yyyyMMdd&quot;.log&quot;"/>
<param name="PrintFlag" value="true" />
<ImmediateFlush value="true" />
<encoding value="utf-8"/>
<layout type="log4net.Layout.PatternLayout">
<param name="ConversionPattern" value="[%p][%d{yyyy/MM/dd HH:mm:ss.fff}] [Thread=%thread] %class(%L) %M - %m%n"/>
</layout>
<filter type="log4net.Filter.LevelRangeFilter">
<param name="LevelMin" value="FATAL"/>
<param name="LevelMax" value="FATAL"/>
</filter>
</appender>
<!--The config of System's default level - ERROR-->
<appender name="ERROR" type="log4net.Appender.RollingFileAppender">
<file value="log/error"/>
<appendToFile value="true"/>
<rollingStyle value="Composite" />
<maxSizeRollBackups value="100" />
<maximumFileSize value="102400KB" />
<staticLogFileName value="false" />
<datePattern value="_yyyyMMdd&quot;.log&quot;"/>
<param name="PrintFlag" value="true" />
<ImmediateFlush value="true" />
<encoding value="utf-8"/>
<layout type="log4net.Layout.PatternLayout">
<param name="ConversionPattern" value="[%p][%d{yyyy/MM/dd HH:mm:ss.fff}] [Thread=%thread] %class(%L) %M - %m%n"/>
</layout>
<filter type="log4net.Filter.LevelRangeFilter">
<param name="LevelMin" value="ERROR"/>
<param name="LevelMax" value="ERROR"/>
</filter>
</appender>
<!--The config of System's default level - WARN-->
<appender name="WARN" type="log4net.Appender.RollingFileAppender">
<file value="log/warn"/>
<appendToFile value="true"/>
<rollingStyle value="Composite" />
<maxSizeRollBackups value="100" />
<maximumFileSize value="102400KB" />
<staticLogFileName value="false" />
<datePattern value="_yyyyMMdd&quot;.log&quot;"/>
<param name="PrintFlag" value="true" />
<ImmediateFlush value="true" />
<encoding value="utf-8"/>
<layout type="log4net.Layout.PatternLayout">
<param name="ConversionPattern" value="[%p][%d{yyyy/MM/dd HH:mm:ss.fff}] [Thread=%thread] %class(%L) %M - %m%n"/>
</layout>
<filter type="log4net.Filter.LevelRangeFilter">
<param name="LevelMin" value="WARN"/>
<param name="LevelMax" value="WARN"/>
</filter>
</appender>
<!--The config of System's default level - INFO-->
<appender name="INFO" type="log4net.Appender.RollingFileAppender">
<file value="log/info"/>
<appendToFile value="true"/>
<rollingStyle value="Composite" />
<maxSizeRollBackups value="100" />
<maximumFileSize value="102400KB" />
<staticLogFileName value="false" />
<datePattern value="_yyyyMMdd&quot;.log&quot;"/>
<param name="PrintFlag" value="true" />
<ImmediateFlush value="true" />
<encoding value="utf-8"/>
<layout type="log4net.Layout.PatternLayout">
<param name="ConversionPattern" value="[%p][%d{yyyy/MM/dd HH:mm:ss.fff}] [Thread=%thread] %class(%L) %M - %m%n"/>
</layout>
<filter type="log4net.Filter.LevelRangeFilter">
<param name="LevelMin" value="INFO"/>
<param name="LevelMax" value="INFO"/>
</filter>
</appender>
<!--The config of System's default level - DEBUG-->
<appender name="DEBUG" type="log4net.Appender.RollingFileAppender">
<file value="log/debug"/>
<appendToFile value="true"/>
<rollingStyle value="Composite" />
<maxSizeRollBackups value="100" />
<maximumFileSize value="102400KB" />
<staticLogFileName value="false" />
<datePattern value="_yyyyMMdd&quot;.log&quot;"/>
<param name="PrintFlag" value="false" />
<ImmediateFlush value="true" />
<layout type="log4net.Layout.PatternLayout">
<param name="ConversionPattern" value="[%p][%d{yyyy/MM/dd HH:mm:ss.fff}] [Thread=%thread] %class(%L) %M - %m%n"/>
</layout>
<filter type="log4net.Filter.LevelRangeFilter">
<param name="LevelMin" value="DEBUG"/>
<param name="LevelMax" value="DEBUG"/>
</filter>
</appender>


<!--The basic config-->
<root name="SystemLogger">
<!--The whole switch.(Whose the level is greater than or equals to this will be display)-->
<level value="ALL" />
<appender-ref ref="DEBUG" />
<appender-ref ref="INFO" />
<appender-ref ref="WARN" />
<appender-ref ref="ERROR" />
<appender-ref ref="FATAL" />

</root>
</log4net>
```

第四步：在程序运行之前，加载log4net的配置

```
log4net.Config.XmlConfigurator.Configure("log4net配置文件的路径");
```

第五步：使用log4net记录日志

```
ILog logger = log4net.LogManager.GetLogger("SystemLogger");

log4net.Info("info");

log4net.Error("error");

.....
```