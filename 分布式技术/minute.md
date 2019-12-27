## 技术文档

### 工具类

#### 内网穿透工具

1、frp 是一个高性能的反向代理应用，可以帮助您轻松地进行内网穿透，对外网提供服务，支持 tcp, http, https 等协议类型，并且 web 服务支持根据域名进行路由转发。

详见网址：[https://www.oschina.net/p/frp](https://www.oschina.net/p/frp)

源码地址：[https://github.com/fatedier/frp](https://github.com/fatedier/frp)

目前，还没有使用过。

2、ngrok 是一个反向代理，通过在公共的端点和本地运行的 Web 服务器之间建立一个安全的通道。ngrok 可捕获和分析所有通道上的流量，便于后期分析和重放。ngrok1.0版本是开源的，ngrok2.0后源码不在开源。这个用了段时间，由于官网服务在国外,国内访问国外速度慢，有时还不能访问。

后来搜罗到了一款由sphynx基于ngrok实现的sunny-ngrok版本，在国内访问速度还不错，至少没有掉线。sphynx提供的服务时完全免费的，可以免费注册，非常感谢sphynx。

目前有这个在进行微信开发，非常方便。

当然可以自己搭建自己的服务，可以参考这个：http://www.sunnyos.com/article-show-48.html

哦，对了要把网址贴出来。

网址：[https://www.ngrok.cc/](https://www.ngrok.cc/)

##### 管理界面:(一般不需要动)

ngrok.cc simon_zhang@tom.com / simon_zhang

##### 客户端下载

https://www.ngrok.cc/

##### 如何启动

./sunny clientid ef27154c2f9783a5 你也可以sunny --help 命令查看。


### 技术类

#### 微信SDK


* 微信api文档：https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1445241432&token=&lang=zh_CN
* 开发者入口 ：http://mp.weixin.qq.com/debug/cgi-bin/sandboxinfo?action=showinfo&t=sandbox/index



1、weixin-java-tools：

Weixin Java Tools 微信公众号/企业号开发Java SDK，这个是目前在项目中正在使用的，目前还不错，文档、demo 都还算不错的。

[weixin-SDK](https://github.com/wechat-group/weixin-java-tools)  

[微信java SDK文档](https://github.com/wechat-group/weixin-java-tools/wiki)

#### redis
[源码](http://www.dumpcache.com/wiki/doku.php?id=about_redis_1)

#### CDN
[bootstrap cdn](http://www.bootcdn.cn/?) 


#### 网络编程
[TCP/IP详解 卷1：协议 ](http://www.52im.net/topic-tcpipvol1.html)

[计算机网络通讯协议关系图]( http://www.52im.net/thread-180-1-1.html)

[高性能网络编程(一)：单台服务器并发TCP连接数到底可以有多少](  http://www.52im.net/thread-561-1-1.html)

[MQTT入门篇 ](http://dataguild.org/?p=6817)


### 

###语言

####JAVA
设计模式:http://blog.csdn.net/zhangerqing/article/details/8194653
http://blog.csdn.net/zhangerqing
http://m.blog.csdn.net/article/details?id=7359385
http://www.cnblogs.com/beijiguangyong/archive/2010/11/15/2302807.html

* [Lombok-消除冗长的 java 代码](http://www.blogjava.net/fancydeepin/archive/2012/07/12/lombok.html)

* java_stream用法 https://www.cnblogs.com/sunjf/p/java_stream.html
####Python


#####Go


##### 分布式
zstack 简介：
ZStack是下一代开源的云计算IaaS（基础架构即服务）软件。[1]  它主要面向的是未来的智能数据中心，通过提供全完善的API来管理包括计算、存储和网络在内的数据中心的各种资源。跟OpenStack相比，ZStack具有易用、稳定、灵活、超高性能等特点。[1-2] 
ZStack可以做到15分钟完成安装部署，版本间5分钟无缝升级，全API交付，零手工配置；可以单节点管理十万物理机、百万级虚拟机，同时响应数万并发API调用；[1] 
在API层面提供SQL级别的查询，拥有单项查询条件超过400万个，组合查询条件为400万阶乘；内建工作流引擎，可以在错误发生时随时回滚，维护系统一致性。[3] 
官网：http://zstack.org/
zstack源码： https://github.com/zstackio/zstack

zbus：

zbus源码：http://git.oschina.net/rushmore/zbus
文章：http://www.cnblogs.com/rwxwsblog/p/5848408.html
文章：http://www.tuicool.com/articles/NnMBBfI
http://www.cnblogs.com/top15from/p/4899954.html


蜘蛛侠： 简单的说，这是一个网页爬虫工具，专门对网页内容进行抓取和解析
http://git.oschina.net/l-weiwei/Spiderman2

#### spring boot 微服
官方文档 https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/

《Spring Cloud微服务实战》作者 http://blog.didispace.com/springcloud1/

Spring Boot基础教程 http://blog.didispace.com/Spring-Boot%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B/

Spring Cloud基础教程 http://blog.didispace.com/Spring-Cloud%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B/

基于Spring Boot和Spring Cloud实现微服务架构 http://blog.csdn.net/zeb_perfect/article/details/51945350

基于Spring Boot、Spring Cloud、Docker的微服务系统架构实践 http://blog.csdn.net/rickiyeat/article/details/60792925

基于Spring Cloud的微服务架构设计 http://www.roncoo.com/article/detail/127741

Microservices https://martinfowler.com/articles/microservices.html
