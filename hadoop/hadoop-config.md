## HDFS

### namenode
1、数据存放在哪儿

hdfs-site.xml

```xml
<property>
    <name>dfs.namenode.name.dir</name>
    <value>/srv/hadoop/namenode</value>
</property>
```



2、服务监听地址

hdfs-site.xml

```xml
<property>
    <name>dfs.namenode.rpc-address</name>
    <value>0.0.0.0:9010</value>
</property>
<property>
    <name>dfs.namenode.servicerpc-address</name>
    <value>0.0.0.0:9011</value>
</property>
<property>
    <name>dfs.namenode.http-address</name>
    <value>0.0.0.0:9012</value>
</property>
<property>
    <name>dfs.namenode.secondary.http-address</name>
    <value>0.0.0.0:9013</value>
</property>
```



3、附加选项

hdfs-site.xml

```xml
<property>
    <name>dfs.namenode.datanode.registration.ip-hostname-check</name>
    <value>false</value>
</property>
<property>
	<name>dfs.permissions</name>
	<value>false</value>
</property>
```



4、伪分布式

伪分布式下namenode和datanode公用配置，则：

1. datanode通过dfs.namenode.rpc-address，知道namenode在哪儿！
2. namenode通过dfs.namenode.rpc-address，指定监听端口；dfs.namenode.rpc-bind-host，指定监听IP

hdfs-site.xml

```xml
<property>
    <name>dfs.namenode.rpc-address</name>
    <value>namenode-ip:9010</value>
</property>
<property>
    <name>dfs.namenode.rpc-bind-host</name>
    <value>0.0.0.0</value>
</property>
```



### datanode

1、数据存放在哪儿

hdfs-site.xml

```xml
<property>
    <name>dfs.datanode.data.dir</name>
    <value>/srv/hadoop/datanode</value>
</property>
```



2、服务监听地址

hdfs-site.xml

```xml
<property>
    <name>dfs.datanode.ipc.address</name>
    <value>0.0.0.0:9020</value>
</property>
<property>
    <name>dfs.datanode.address</name>
    <value>0.0.0.0:9021</value>
</property>
<property>
    <name>dfs.datanode.http.address</name>
    <value>0.0.0.0:9022</value>
</property>
```



3、namenode在哪儿

hdfs-site.xml

```xml
<property>
    <name>dfs.namenode.rpc-address</name>
    <value>namenode-ip:9010</value>
</property>
```



4、网页中显示的主机名

hdfs-site.xml

```xml
<property>
    <name>dfs.datanode.hostname</name>
    <value>datanode-ip</value>
</property>
```





### start-dfs

1、namenode在哪儿

hdfs-site.xml

```xml
<property>
    <name>dfs.namenode.rpc-address</name>
    <value>namenode-ip:9010</value>
</property>
```



2、namenode.secondary在哪儿

hdfs-site.xml

```xml
<property>
    <name>dfs.namenode.secondary.http-address</name>
    <value>namenode-ip:9013</value>
</property>
```



3、datanode在哪儿
$HADOOP_HOME/etc/hadoop/workers



### hdfs client

1、默认文件系统(yarn相关服务必须配置，否则会出现各种问题)
core-site.xml

```xml
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://namenode-ip:9010</value>
</property>
```



2、每个文件备份数
hdfs-site.xml

```xml
<property>
    <name>dfs.replication</name>
    <value>1</value>
</property>
```



## YARN

### resourcemanager
1、服务监听地址

```xml
<property>
    <name>yarn.resourcemanager.address</name>
    <value>0.0.0.0:9030</value>
</property>
<property>
    <name>yarn.resourcemanager.admin.address</name>
    <value>0.0.0.0:9031</value>
</property>
<property>
    <name>yarn.resourcemanager.webapp.address</name>
    <value>0.0.0.0:9032</value>
</property>
<property>
    <name>yarn.resourcemanager.scheduler.address</name>
	<value>0.0.0.0:9033</value>
</property>
<property>
    <name>yarn.resourcemanager.resource-tracker.address</name>
    <value>0.0.0.0:9034</value>
</property>
```



### nodemanager

1、nodemanager监听地址

```xml
<property>
    <name>yarn.nodemanager.address</name>
    <value>0.0.0.0:9040</value>
</property>
<property>
    <name>yarn.nodemanager.localizer.address</name>
    <value>0.0.0.0:9041</value>
</property>
<property>
    <name>yarn.nodemanager.webapp.address</name>
    <value>0.0.0.0:9042</value>
</property>
```



2、resourcemanager在哪儿

```xml
<property>
    <name>yarn.resourcemanager.resource-tracker.address</name>
    <value>resourcemanager-ip:9034</value>
</property>
```



3、辅助配置选项

```xml
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
```



4、让resourcemanager能访问自己

方案一、

修改resourcemanager中的/etc/hosts文件，添加`namenode-ip	namenode-host`

方案二、

修改nodemanager中的/etc/hosts文件，添加`namenode-ip namenode-ip`

这种方式会让网络界面上显示的东西被修改





### start-yarn

只能在resourcemanager上启动集群

文件`$HADOOP_HOME/etc/hadoop/workers`中获取namenode主机。




## MAPREDUCE

### jobhistory

1、监听地址

```xml
<property>
     <name>mapreduce.jobhistory.address</name>
     <value>0.0.0.0:9050</value>
</property>
<property>
     <name>mapreduce.jobhistory.admin.address</name>
     <value>0.0.0.0:9051</value>
</property>
<property>
        <name>mapreduce.jobhistory.webapp.address</name>
    <value>0.0.0.0:9052</value>
</property>
```



2、日志聚合功能

如果要启用日志聚合功能，则使用该配置；选项位于nodemanager的yarn-site.xml中。

```xml
<property>
    <name>yarn.log-aggregation-enable</name>
	<value>true</value>
</property>
```



### mapreduce

1、mapreduce配置

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>yarn.app.mapreduce.am.env</name>
        <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    </property>
    <property>
        <name>mapreduce.map.env</name>
        <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    </property>
    <property>
        <name>mapreduce.reduce.env</name>
        <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    </property>
    
    <!--如果没有该服务，分布式下报异常，但已经计算完成-->
    <property>
     	<name>mapreduce.jobhistory.address</name>
     	<value>0.0.0.0:9050</value>
	</property>
</configuration>
```



2、resourcemanager定位

```xml
<property>
    <name>yarn.resourcemanager.address</name>
    <value>resourcemanager-ip:9030</value>
</property>
<property>
    <name>yarn.resourcemanager.scheduler.address</name>
    <value>resourcemanager-ip:9033</value>
</property>
```

