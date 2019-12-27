## Redis3.2.6 安装及分布式配置

参考[redis3.0.3 安装与配置](http://blog.csdn.net/miyatang/article/details/47257209)

### 1、安装

####1.1 mac 上安装redis

1）官网 https://redis.io/ 下载最新的稳定版本，我下的是3.2.6

2）sudo tar -zxf redis-3.2.6.zip 解压文件

3）进入解压后的目录 cd redis－3.2.6

4)、sudo make test 测试编译

```
  21 seconds - integration/convert-zipmap-hash-on-load
  85 seconds - unit/dump
  8 seconds - unit/limits
  14 seconds - unit/introspection-2
  78 seconds - integration/replication-2
  93 seconds - unit/sort
  14 seconds - unit/bitfield
  100 seconds - unit/type/list-3
  41 seconds - unit/maxmemory
  43 seconds - unit/bitops
  56 seconds - unit/scripting
  128 seconds - unit/type/list-2
  43 seconds - unit/memefficiency
  135 seconds - integration/replication
  79 seconds - unit/obuf-limits
  148 seconds - integration/replication-3
  75 seconds - unit/hyperloglog
  162 seconds - integration/replication-4
  104 seconds - unit/geo
  182 seconds - integration/replication-psync

\o/ All tests passed without errors!

Cleanup: may take some time... OK
```


5)、sudo make install

```
localhost:redis-3.2.6 xingzhe$ sudo make install
Password:
cd src && /Applications/Xcode.app/Contents/Developer/usr/bin/make install

Hint: It's a good idea to run 'make test' ;)

    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install
```

至此，你已成功安装redis！

####1.2 centos7 上安装redis

1)、安装前，升级系统
 ```
 yum -y update
 ```
2)、官网下载
```
wget http://download.redis.io/releases/redis-3.2.6.tar.gz
```
3)、解压
```
tar -xvf redis-3.2.6.tar.gz redis-3.2.6
```
4)、进入解压后的目录
```
cd redis-3.2.6
```
5)、运行runtest
```
# ./runtest
 You need tcl 8.5 or newer in order to run the Redis test
```
如果提示 需要安装tcl，执行6步，否则跳过。
6)、安装tcl
```
# yum install tcl
```

7)、编译
```
# make
cd src && make all
make[1]: 进入目录“/tdata/redis-3.2.6/src”
rm -rf redis-server redis-sentinel redis-cli redis-benchmark redis-check-rdb redis-check-aof *.o *.gcda *.gcno *.gcov redis.info lcov-html
(cd ../deps && make distclean)
make[2]: 进入目录“/tdata/redis-3.2.6/deps”
(cd hiredis && make clean) > /dev/null || true
(cd linenoise && make clean) > /dev/null || true
(cd lua && make clean) > /dev/null || true
(cd geohash-int && make clean) > /dev/null || true
(cd jemalloc && [ -f Makefile ] && make distclean) > /dev/null || true
(rm -f .make-*)
make[2]: 离开目录“/tdata/redis-3.2.6/deps”
(rm -f .make-*)
echo STD=-std=c99 -pedantic -DREDIS_STATIC='' >> .make-settings
echo WARN=-Wall -W >> .make-settings
echo OPT=-O2 >> .make-settings
echo MALLOC=jemalloc >> .make-settings
echo CFLAGS= >> .make-settings
echo LDFLAGS= >> .make-settings
echo REDIS_CFLAGS= >> .make-settings
echo REDIS_LDFLAGS= >> .make-settings
echo PREV_FINAL_CFLAGS=-std=c99 -pedantic -DREDIS_STATIC='' -Wall -W -O2 -g -ggdb   -I../deps/geohash-int -I../deps/hiredis -I../deps/linenoise -I../deps/lua/src -DUSE_JEMALLOC -I../deps/jemalloc/include >> .make-settings
echo PREV_FINAL_LDFLAGS=  -g -ggdb -rdynamic >> .make-settings
(cd ../deps && make hiredis linenoise lua geohash-int jemalloc)
make[2]: 进入目录“/tdata/redis-3.2.6/deps”
(cd hiredis && make clean) > /dev/null || true
(cd linenoise && make clean) > /dev/null || true
(cd lua && make clean) > /dev/null || true
(cd geohash-int && make clean) > /dev/null || true
(cd jemalloc && [ -f Makefile ] && make distclean) > /dev/null || true
(rm -f .make-*)
(echo "" > .make-cflags)
(echo "" > .make-ldflags)
MAKE hiredis
cd hiredis && make static
make[3]: 进入目录“/tdata/redis-3.2.6/deps/hiredis”
gcc -std=c99 -pedantic -c -O3 -fPIC  -Wall -W -Wstrict-prototypes -Wwrite-strings -g -ggdb  net.c
make[3]: gcc：命令未找到
make[3]: *** [net.o] 错误 127
make[3]: 离开目录“/tdata/redis-3.2.6/deps/hiredis”
make[2]: *** [hiredis] 错误 2
make[2]: 离开目录“/tdata/redis-3.2.6/deps”
make[1]: [persist-settings] 错误 2 (忽略)
    CC adlist.o
/bin/sh: cc: 未找到命令
make[1]: *** [adlist.o] 错误 127
make[1]: 离开目录“/tdata/redis-3.2.6/src”
make: *** [all] 错误 2
```
若出现上面错误，说明缺少编译环境，需要安装gcc。否则跳过8步。
如果出现如下错误,请看第9步，否则跳过。
```
cd src && make all
make[1]: 进入目录“/tdata/redis-3.2.6/src”
    CC adlist.o
In file included from adlist.c:34:0:
zmalloc.h:50:31: 致命错误：jemalloc/jemalloc.h：没有那个文件或目录
 #include <jemalloc/jemalloc.h>
                               ^
编译中断。
make[1]: *** [adlist.o] 错误 1
make[1]: 离开目录“/tdata/redis-3.2.6/src”
make: *** [all] 错误 2

```
8)、安装gcc
```
# yum install gcc
```
安装成功转到6步，执行make命令。
9)、zmalloc.h:50:31: 致命错误：jemalloc/jemalloc.h：没有那个文件或目录
在make 后面增加 MALLOC=libc
```
# make MALLOC=libc
```
10)、安装
```
# make install
```
注：在编译时，出现“../deps/geohash-int/geohash.h:78:18: 附注：‘longitude’在此声明” 警告；还有一个变量未定义。不清楚是否会有影响。目前，安装后运行测试都正常。


[Redis Desktop Manager](https://github.com/uglide/RedisDesktopManager/releases/download/0.8.8/redis-desktop-manager-0.8.8.384.exe)


### 验证

1)、启动服务

```
 redis-server redis.conf
```
 [Redis 安装 启动 连接 配置 重启](http://www.cnblogs.com/GoQC/p/5764201.html)

2)、测试

```
$ redis-cli
127.0.0.1:6379> set foo bar
OK
127.0.0.1:6379> get foo
"bar"
```

恭喜你，已经成功安装！


### 2、分布式配置

参考[1]:[Redis Cluster搭建方法简介](http://www.redis.cn/topics/cluster-tutorial.html)

参考[2]:[ Centos7 搭建Redis3.2.0版本集群环境](http://blog.csdn.net/weiguolong0306/article/details/51586557)


准备ruby环境,后续redis-trib.rb会用到。
```
# yum install ruby
# yum install rubygems
# gem install redis
```




#### 搭建并使用Redis集群[]
1、准备redis.conf配置文件
redis.conf配置如下:
```
port 7000
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes
```

2、创建目录后分别启动
创建目录
```
mkdir cluster-test
cd cluster-test
mkdir 7000 7001 7002 7003 7004 7005
```
在文件夹 7000 至 7005 中， 各创建一个 redis.conf 文件,文件中的端口与文件加相同。
启动,因文件夹加多启动比较繁琐。可用写个简单的脚本执行,注意一定是在文件夹内执行命令。
vi runcluster.sh 将下面内容cp进去。
```
cd 7000/
../redis-server ./redis.conf &
cd ../7001
../redis-server ./redis.conf &
cd ../7002
../redis-server ./redis.conf &
cd ../7003
../redis-server ./redis.conf &
cd ../7004
../redis-server ./redis.conf &
cd ../7005
../redis-server ./redis.conf &
```
nohup redis-server ./redis.conf > /cluster-test/log/redis-7000.log 2>&1 &

chomd +X runcluster.sh //添加执行权限
然后,./runcluster.sh  运行。这样就全都启动了。

3、搭建集群
redis-trib 位于 Redis 源码的 src 文件夹中,你要把它cp到cluster-test目录下。

```
./redis-trib.rb create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005
```
注意:在执行时会出现"All nodes agree about slots configuration"提示,不能直接回车,一定输入yes再回车。
当出现下面的说明配置完成。
```
12400:S 12 Jan 14:45:21.671 * Background AOF rewrite finished successfully
12397:M 12 Jan 14:45:22.055 # Cluster state changed: ok
12398:M 12 Jan 14:45:22.155 # Cluster state changed: ok
12399:M 12 Jan 14:45:22.189 # Cluster state changed: ok
```

4、检验是否配置成功
./redis-trib.rb check :7000
你也可用通过客户端连接上测试
```
redis-cli -c -p 7000
127.0.0.1:7000> cluster nodes
1577e1769a77367dd4a626b316f4191224f726c6 127.0.0.1:7005 slave 8da8517b0edd30f30104e091d782d40803c0723d 0 1484203764628 6 connected
7436728328935447bccc1c1b54e6eab372b8c4e1 127.0.0.1:7000 myself,master - 0 0 1 connected 0-5460
8da8517b0edd30f30104e091d782d40803c0723d 127.0.0.1:7002 master - 0 1484203766133 3 connected 10923-16383
3a3e3c8407ee19f9f762c3a7496fc0d213ee452d 127.0.0.1:7001 master - 0 1484203765631 2 connected 5461-10922
48302b26e73b33da61914a33d62865ba0e83f6cb 127.0.0.1:7003 slave 7436728328935447bccc1c1b54e6eab372b8c4e1 0 1484203764127 4 connected
5b75f1ce4b70e3a780016d3a77daea7adde432ce 127.0.0.1:7004 slave 3a3e3c8407ee19f9f762c3a7496fc0d213ee452d 0 1484203765129 5 connected
127.0.0.1:7000> cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_sent:1169
cluster_stats_messages_received:1169
127.0.0.1:7000> set name hello
-> Redirected to slot [5798] located at 127.0.0.1:7001
OK

redis-cli -c -p 7003
127.0.0.1:7003> get name
-> Redirected to slot [5798] located at 127.0.0.1:7001
"hello"
```

5、 [redis集群设置密码](http://blog.csdn.net/daiyudong2020/article/details/51674169)
注意事项：
1.如果是使用redis-trib.rb工具构建集群，集群构建完成前不要配置密码，集群构建完毕再通过config set + config rewrite命令逐个机器设置密码
2.如果对集群设置密码，那么requirepass和masterauth都需要设置，否则发生主从切换时，就会遇到授权问题，可以模拟并观察日志
3.各个节点的密码都必须一致，否则Redirected就会失败
```
config set masterauth abc
config set requirepass abc
config rewrite
```

config set masterauth "simonzhang"
config set requirepass "simonzhang"
config rewrite

密码方式访问：

redis-cli -p 7000 -a abc   //abc is password

```
#客户端关闭
redis-cli -h 127.0.0.1 -p 6379 shutdown
redis-cli -h 127.0.0.1 -p 6380 shutdown
#启动
redis-server redis6379.conf &
redis-server redis6380.conf &
```
??? 主从切换
[](http://www.2cto.com/database/201502/377061.html)




### 3、搭建前哨sentinel
配置
redis-sentinel ./sentinel001.conf &
redis-sentinel ./sentinel002.conf &
redis-sentinel ./sentinel003.conf &

注：sentinel.conf中要指定redis服务器的网络ip地址.不能为本机地址（127.0.0.1/localhost）。这是在单机部署时一定要注意的。


在有sentinel的集群环境中报：
```
DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from external computers to Redis you may adopt one of the following solutions:
1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent.
2) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server.
3) If you started the server manually just for testing, restart it with the '--protected-mode no' option.
4) Setup a bind address or an authentication password.
 NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.
```

### 4、主从配置

master 部署在192.168.0.99上，配置文件为redis.conf;slave部署在192.168.0.100上，从master上复制一份配置文件命名为redis-slave.conf,如果你是单机部署需要修改ip和端口号。
再在redis-slav.conf配置文件中增加：
```
slaveof 192.168.0.99 6379
```

### 5、配置文件

http://blog.csdn.net/u012173245/article/details/52041882
http://yijiebuyi.com/blog/bc2b3d3e010bf87ba55267f95ab3aa71.html
https://raw.githubusercontent.com/antirez/redis/3.0/redis.conf

#### 示例：
```
#修改为守护模式
daemonize yes
#设置进程锁文件
pidfile /usr/local/redis/redis.pid
#端口
port 6379
#客户端超时时间
timeout 300
#日志级别
loglevel debug
#日志文件位置
logfile /usr/local/redis/log-redis.log
#设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
databases 8
##指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
#save <seconds> <changes>
#Redis默认配置文件中提供了三个条件：
save 900 1
save 300 10
save 60 10000
#指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，
#可以关闭该#选项，但会导致数据库文件变的巨大
rdbcompression yes
#指定本地数据库文件名
dbfilename dump.rdb
#指定本地数据库路径
dir /usr/local/redis/db/
#指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能
#会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有
#的数据会在一段时间内只存在于内存中
appendonly no
#指定更新日志条件，共有3个可选值：
#no：表示等操作系统进行数据缓存同步到磁盘（快）
#always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）
#everysec：表示每秒同步一次（折衷，默认值）
appendfsync everysec
```

### 6、文档

* [Redis 命令参考](http://doc.redisfans.com/#)
