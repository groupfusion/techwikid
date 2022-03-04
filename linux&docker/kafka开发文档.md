# kafka开发文档
## 1.kafka单机版安装for Windows
 参考[在Windows下安装使用Kafka](https://www.jianshu.com/p/ce203d4e2f41)

###1.1准备工作
	1.Java环境
	2.Kafka安装包（已包含zookeeper）
### 1.2安装步骤
	1.Java安装 自行百度 
    2.下载、安装Kafka
 		a.打开 [下载地址](http://kafka.apache.org/downloads.html)
 		b.选择合适的版本，Kafka包名组成： Scala版本 - Kafka自身版本
 		c.下载完成之后解压。
### 1.3启动服务
	
#### 1.3.1启动ZooKeeper

打开kafka_2.12-2.8.0\bin\windows目录(这里用的是kafka_2.12-2.8.0这个版本)，该目录下是所有windows命令。

在此目录下打开cmd，执行命令 zookeeper-server-start.bat ..\..\config\zookeeper.properties

#### 1.3.2 启动Kafka
依旧在目录下打开cmd，执行命令 kafka-server-start.bat ..\..\config\server.properties

启动成功。
注意：
如果出现‘命令语法不正确’ ，导致不能正常运行，尝试修改配置文件的dataDir（zookeeper.properties），log.dirs（server.properties）。因为默认的是linux的文件目录格式。

### 1.4测试Kafka命令

#### 1.4.1创建一个主题
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic kafka-test-topic
#### 1.4.2查看创建的主题列表
kafka-topics.bat --list --zookeeper localhost:2181
执行完上面两条命令.

#### 1.4.3启动生产者：
kafka-console-producer.bat --broker-list localhost:9092 --topic kafka-test-topic
此时可以从控制台输入信息，待消费者启动后可接收到生产者发布的消息。

启动消费者：
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic kafka-test-topic --from-beginning
此时便能看到发布出去的消息了

## 2 kafka-eagle
给kafka配一个web版的管理页面或仪表盘，管理起来更加方便
参考[kafka管理界面 kafka-eagle](https://www.jianshu.com/p/db9f37bb7f98),[kafka-eagle](https://www.jianshu.com/p/fabd732c0d0a)
### 2.1 环境说明
kafka已安装
见文档：[《kafka单机启动》](https://www.jianshu.com/p/97680145ca82) 、 [《kafka集群搭建》](https://www.jianshu.com/p/514900a097d8)

### 2.2 文件准备
1.下载地址
[https://www.kafka-eagle.org](https://www.kafka-eagle.org/)


2.文件准备
将文件拷贝到服务器，解压缩至 /data 目录里
在opt下创建软连接
```
# ln -s /data/kafka-eagle-bin-2.0.5/kafka-eagle-web-2.0.5 /opt/kafka-eagle
```
### 2.3. 修改kafka
修改之前安装的kafka

1.修改配置文件
kafka-eagle要连接kafka的9999端口，我们需要打开kafka的这个端口。
在配置文件/opt/kafka/config/server.properties中添加如下内容 export JMX_PORT="9999"

```
if [ "x$KAFKA_HEAP_OPTS" = "x" ]; then
    export KAFKA_HEAP_OPTS="-Xmx1G -Xms1G"
    export JMX_PORT="9999"
fi
```
2.重启kafka
```
# /opt/kafka/bin/kafka-server-stop.sh
# nohup /opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties 1>/dev/null 2>&1 &
```
3.查看结果

```
[root@kafka bin]# netstat -ntlp|grep java
tcp6       0      0 :::9999                 :::*                    LISTEN      8202/java
tcp6       0      0 :::36218                :::*                    LISTEN      5595/java
tcp6       0      0 :::39747                :::*                    LISTEN      8202/java
tcp6       0      0 10.10.239.41:9092       :::*                    LISTEN      8202/java
tcp6       0      0 :::38788                :::*                    LISTEN      8202/java
tcp6       0      0 :::2181                 :::*                    LISTEN      5595/java
```

### 2.4. 修改 kafka-eagle 配置文件
修改配置文件 /opt/kafka-eagle/conf/system-config.properties

1.zookeeper集群连接
> 下边是一个单机kafka测试环境的zookeeper

```
######################################
# multi zookeeper & kafka cluster list
######################################
kafka.eagle.zk.cluster.alias=cluster1
cluster1.zk.list=127.0.0.1:2181
#cluster2.zk.list=xdn10:2181,xdn11:2181,xdn12:2181
```
> 下边是一个kafka的集群环境中zookeeper集群

```
######################################
# multi zookeeper & kafka cluster list
######################################
kafka.eagle.zk.cluster.alias=cluster1
cluster1.zk.list=10.10.239.61:2181,10.10.239.62:2181,10.10.239.63:2181
#cluster2.zk.list=xdn10:2181,xdn11:2181,xdn12:2181
```

2.kafka连接配置

```
######################################
# kafka sasl authenticate
######################################
cluster1.kafka.eagle.sasl.enable=true  # 修改为true
cluster1.kafka.eagle.sasl.protocol=SASL_PLAINTEXT
cluster1.kafka.eagle.sasl.mechanism=PLAIN
cluster1.kafka.eagle.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="liubei" password="liubei@2021";    #修改kafka的账号密码
cluster1.kafka.eagle.sasl.client.id=
cluster1.kafka.eagle.blacklist.topics=
cluster1.kafka.eagle.sasl.cgroup.enable=false
cluster1.kafka.eagle.sasl.cgroup.topics=
```

3.修改端口

```
######################################
# kafka eagle webui port
######################################
kafka.eagle.webui.port=80
```

4.修改数据库和认证

```
######################################
# kafka ssl authenticate
######################################
cluster3.kafka.eagle.ssl.enable=false
cluster3.kafka.eagle.ssl.protocol=SSL
#修改下边一行，数据库数据目录的位置，kafka-eagle目录下默认有db这个目录
cluster3.kafka.eagle.ssl.truststore.location=jdbc:sqlite:/opt/kafka-eagle/db/ke.db
cluster3.kafka.eagle.ssl.truststore.password=
cluster3.kafka.eagle.ssl.keystore.location=
cluster3.kafka.eagle.ssl.keystore.password=
cluster3.kafka.eagle.ssl.key.password=
cluster3.kafka.eagle.ssl.endpoint.identification.algorithm=https
cluster3.kafka.eagle.blacklist.topics=
cluster3.kafka.eagle.ssl.cgroup.enable=false
cluster3.kafka.eagle.ssl.cgroup.topics=

######################################
# kafka sqlite jdbc driver address
######################################
kafka.eagle.driver=org.sqlite.JDBC
##修改下边一行，数据库数据目录的位置（同上）
kafka.eagle.url=jdbc:sqlite:/opt/kafka-eagle/db/ke.db
#数据库用户名密码，改不改都行
kafka.eagle.username=liubei
kafka.eagle.password=liubei@2021
```

> 当然你也可以使用mysql，配置文件最后有mysql的配置，打开并配置，再注释掉jdbs即可

### 2.5. 启动kafka-eagle
1.配置环境变量
如果不想每次到目录底下启动服务，可以在PATH变量中指定路径，写到系统变量还是用户变量中看自己需要，我们写到全局变量中，在/etc/profile文件中添加如下内容：
```
#############kafka-eagle##################
export KE_HOME=/opt/kafka-eagle
export PATH=$PATH:$KE_HOME/bin
```
2.启动服务
```
# ke.sh start
```
> 成功后输出如下
```
[2021-08-05 14:40:39] INFO: Port Progress: [##################################################] | 100%
[2021-08-05 14:40:42] INFO: Config Progress: [##################################################] | 100%
[2021-08-05 14:40:46] INFO: Startup Progress: [##################################################] | 100%
[2021-08-05 14:40:36] INFO: Status Code[0]
[2021-08-05 14:40:36] INFO: [Job done!]
Welcome to
    __ __    ___     ____    __ __    ___            ______    ___    ______    __     ______
   / //_/   /   |   / __/   / //_/   /   |          / ____/   /   |  / ____/   / /    / ____/
  / ,<     / /| |  / /_    / ,<     / /| |         / __/     / /| | / / __    / /    / __/
 / /| |   / ___ | / __/   / /| |   / ___ |        / /___    / ___ |/ /_/ /   / /___ / /___
/_/ |_|  /_/  |_|/_/     /_/ |_|  /_/  |_|       /_____/   /_/  |_|\____/   /_____//_____/


Version 2.0.5 -- Copyright 2016-2021
*******************************************************************
* Kafka Eagle Service has started success.
* Welcome, Now you can visit 'http://127.0.0.1:8048'
* Account:admin ,Password:123456
*******************************************************************
* <Usage> ke.sh [start|status|stop|restart|stats] </Usage>
* <Usage> https://www.kafka-eagle.org/ </Usage>
*******************************************************************
```
> 用户名和url上边已经给输出了，当然访问的时候要把回环地址替换掉。

### 2.6. 结果查看

1.查看端口

```
[root@kafka-01 ~]# netstat -ntlp|grep java
tcp6       0      0 :::9999                 :::*                    LISTEN      156341/java
tcp6       0      0 :::80                   :::*                    LISTEN      222701/java
tcp6       0      0 10.10.239.61:3888       :::*                    LISTEN      1396/java
tcp6       0      0 :::8080                 :::*                    LISTEN      1396/java
tcp6       0      0 :::45527                :::*                    LISTEN      1396/java
tcp6       0      0 127.0.0.1:8065          :::*                    LISTEN      222701/java
tcp6       0      0 10.10.239.61:9092       :::*                    LISTEN      156341/java
tcp6       0      0 :::8069                 :::*                    LISTEN      222701/java
tcp6       0      0 :::2181                 :::*                    LISTEN      1396/java
tcp6       0      0 :::44774                :::*                    LISTEN      156341/java
tcp6       0      0 :::41354                :::*                    LISTEN      156341/java
```
2.web访问
> dashboard
