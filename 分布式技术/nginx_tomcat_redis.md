## nginx+tomcat+redis实现集群环境的session共享。

注：tomcat-redis-session-manager 仅对tomcat8.0.39以下版支持。8.5.9这个版本不支持，缺少LifecycleSupport 相关类。

### nginx tomcat 集群配置
参考：

[Nginx+Tomcat7+Mencached负载均衡集群部署笔记](http://blog.csdn.net/zht666/article/details/38515147)

[Nginx-tomcat-redis------负载均衡以及session共享](http://www.cnblogs.com/chihirotan/p/5791401.html)

nginx\tomcat 采用集群方式，部署在一台window机器上192.168.0.20。redis 部署在一台centos下，ip为192.168.0.21；

配置步骤如下：
1、安装nginx及单机测试，这里就不多说了，不会的同学可以百度下，很多。
2、安装tomcat 从官网下载tomcat7。注：在同一机器上部署需要修改端口，否则可能因端口冲突无法启动；我的端口分别设置为8080，8081，8082
3、nginx集群配置
修改nginx安装目录下的conf/nginx.conf 文件。修改部分如下：

```
 http{
    #设定负载均衡的服务器列表，可以设置多个upstream，但mysvr名字要区分
    upstream myClusterServer1 {
       #weigth参数表示权值，权值越高被分配到的几率越大
       #本机上的Squid开启3128端口
       server 127.0.0.1:8080  weight=5;
       server 127.0.0.1:8081  weight=5;
       server 127.0.0.1:8082  weight=5;
    }
    server {
        ...
        location / {
            root   html;
            index  index.html index.htm index.jsp;
            #请求转向mysvr 定义的服务器列表
            proxy_pass    http://myClusterServer1;
            proxy_redirect default;
            #跟代理服务器连接的超时时间，必须留意这个time out时间不能超过75秒，当一台服务器当掉时，过10秒转发到另外一台服务器。
        	proxy_connect_timeout 10;
        }
        ...
    }
    ....
 }
```
4、启动3个tomcat和nginx。
5、测试，编写一个简单jsp进行测试。放到tomcat下，然后[http://localhost/test.jsp](http://localhost/test.jsp)访问,
```
<?xml version="1.0" encoding="UTF-8"?>
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
<head>
    <meta http-equiv="content-type" content="text/html; charset=iso-8859-1"/>
    <meta http-equiv="pragma" content="no-cache"/><!-- HTTP 1.0 -->
</head>
<body>
   <h1>Session</h1>
   <table style="text-align: left;" border="0">
     <tr>
       <th>Session Id</th>
       <td><%=request.getSession().hashCode()%></td>
     </tr>
     <tr>
       <th>Session IP</th>
       <td><%=request.getServerName()%></td>
     </tr>
     <tr>
       <th>Session port</th>
       <td><%=request.getServerPort()%></td>
     </tr>
     <tr>
       <th>server</th>
       <td>this is Tomcat Server 8080</td>
     </tr>
   </table>
</body>
</html>
```

### redis实现session共享
参考：[用Redis存储Tomcat集群的Session](http://blog.csdn.net/chszs/article/details/42610365)

redis安装这里就不多说了。参见我的[redis3.2.6_安装及分布配置]()

1、准备tomcat-redis-session-manager包

注：tomcat-redis-session-manager仅支持tomcat8.0.39以下版本，并按网上提供的方法修改时没有问题的。对8.5以上版本依然没用。

你要到[tomcat-redis-session-manager](https://github.com/jcoleman/tomcat-redis-session-manager)下载源码编译。

在配置gradle时会遇到“No such property: sonatypeUsername错误”

解决方法：
在项目根目录下创建gradle.properties文件，文件中添加如下内容：

```
sonatypeUsername=
sonatypePassword=
sonatypeRepo=
```
编译成功后将commons-pool2-2.4.2.jar、jedis-2.9.0.jar、tomcat-redis-session-manager-2.0.0.jar 放到tomcat的lib目录下。

2、在tomcat context.xml 文件中增加先内容。

单机redis缓存（master -slave模式）

```
    <!--单点配置-->
    <Valve className="com.orangefunction.tomcat.redissessions.RedisSessionHandlerValve" />
    <Manager className="com.orangefunction.tomcat.redissessions.RedisSessionManager"
             host="192.168.100.194"
             port="6379"
             database="0"
             maxInactiveInterval="60"
             />
```

Sentinel 配置

```
    <!-- Sentinel 配置 -->
    <Valve className="com.orangefunction.tomcat.redissessions.RedisSessionHandlerValve" />
    <Manager className="com.orangefunction.tomcat.redissessions.RedisSessionManager"
      maxInactiveInterval="60"
	  sentinelMaster="mymaster"
      sentinels="192.168.0.194:26379,192.168.0.194:36379,192.168.0.194:46379"/>
```

3、编写测试jsp。
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <%=session.getId()%><br>
    <%
        String msg = (String) session.getAttribute("msg");
        if (null == msg) {
            session.setAttribute("msg", "Hello!");
        } else {
            session.setAttribute("msg", msg + 0);
        }
    %>
    <%=session.getAttribute("msg")%>
</body>
</html>
```
分别部署到tomcat下

4、启动nginx和tomcat

分别启动tomcat和nginx后，然后访问[http://localhost/test.jsp](http://localhost/test.jsp)

```
B1ACBA61EEB15371A3896FCEA5FAF69D
Hello!0000
```

若出现DENIED Redis is running in protected mode because protected mode is enabled问题
具体提示如下：
```
redis.clients.jedis.exceptions.JedisDataException: DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from external computers to Redis you may adopt one of the following solutions:
1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent.
2) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server.
3) If you started the server manually just for testing, restart it with the '--protected-mode no' option.
4) Setup a bind address or an authentication password. NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.
```
你可以根据你的实际情况做出处理。
我这里是临时测试，在启动时把protected-mode 设置为no
```
# redis-server ./redis.conf --protected-mode no
```

如果生产环境建议绑定一个地址或通过密码访问。

单机方式下上面的方式很容易解决。但集群方式存在一个很大的坑， one client（nginx+tomcat 集群） + one server（redis+sentinel集群）下。无论是关闭protected-mode 还是 bind address 和auth pass 都无效。

redis集群方式配置失败！！！！


参考：
[redis + Tomcat 8 的session共享解决](http://www.cnblogs.com/interdrp/p/4868740.html)

分布式集群Session共享 简单多tomcat8+redis的session共享实现
http://blog.csdn.net/jerome_s/article/details/52658946
[用Redis存储Tomcat集群的Session](http://blog.csdn.net/chszs/article/details/42610365)
[02.Redis主从集群的Sentinel配置](http://www.cnblogs.com/LiZhiW/p/4851631.html#_label2)
[Redis Sentinel配置小记](http://debugo.com/redis-sentinel/?utm_source=tuicool&utm_medium=referral)
[sentinel.conf配置详解](http://www.cnblogs.com/LiZhiW/p/4851640.html)