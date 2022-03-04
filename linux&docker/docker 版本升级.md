# docker 版本升级

## 参考：

[如何将Docker升级到最新版本](https://blog.csdn.net/qq_39629343/article/details/80168084)
[如何将Docker升级到最新版本](https://www.cnblogs.com/PatrickLiu/p/13901520.html)

## 升级过程：
``` sh
[root@dev002 ~]# rpm -qa | grep docker
docker-client-1.13.1-103.git7f2769b.el7.centos.x86_64
docker-1.13.1-103.git7f2769b.el7.centos.x86_64
docker-common-1.13.1-103.git7f2769b.el7.centos.x86_64
[root@dev002 ~]# yum remove docker-1.13.1-103.git7f2769b.el7.centos.x86_64
已加载插件：fastestmirror
正在解决依赖关系
--> 正在检查事务
---> 软件包 docker.x86_64.2.1.13.1-103.git7f2769b.el7.centos 将被 删除
--> 解决依赖关系完成

依赖关系解决

=================================================================================================================================================================================
 Package                            架构                               版本                                                            源                                   大小
=================================================================================================================================================================================
正在删除:
 docker                             x86_64                             2:1.13.1-103.git7f2769b.el7.centos                              @extras                              65 M

事务概要
=================================================================================================================================================================================
移除  1 软件包

安装大小：65 M
是否继续？[y/N]：y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在删除    : 2:docker-1.13.1-103.git7f2769b.el7.centos.x86_64                                                                                                             1/1 
警告：/etc/sysconfig/docker-storage 已另存为 /etc/sysconfig/docker-storage.rpmsave
警告：/etc/docker/daemon.json 已另存为 /etc/docker/daemon.json.rpmsave
  验证中      : 2:docker-1.13.1-103.git7f2769b.el7.centos.x86_64                                                                                                             1/1 

删除:
  docker.x86_64 2:1.13.1-103.git7f2769b.el7.centos                                                                                                                               

完毕！
[root@dev002 ~]# yum remove docker-client-1.13.1-103.git7f2769b.el7.centos.x86_64
已加载插件：fastestmirror
正在解决依赖关系
--> 正在检查事务
---> 软件包 docker-client.x86_64.2.1.13.1-103.git7f2769b.el7.centos 将被 删除
--> 解决依赖关系完成

依赖关系解决

=================================================================================================================================================================================
 Package                                 架构                             版本                                                           源                                 大小
=================================================================================================================================================================================
正在删除:
 docker-client                           x86_64                           2:1.13.1-103.git7f2769b.el7.centos                             @extras                            13 M

事务概要
=================================================================================================================================================================================
移除  1 软件包

安装大小：13 M
是否继续？[y/N]：y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在删除    : 2:docker-client-1.13.1-103.git7f2769b.el7.centos.x86_64                                                                                                      1/1 
  验证中      : 2:docker-client-1.13.1-103.git7f2769b.el7.centos.x86_64                                                                                                      1/1 

删除:
  docker-client.x86_64 2:1.13.1-103.git7f2769b.el7.centos                                                                                                                        

完毕！
[root@dev002 ~]# yum remove docker-common-1.13.1-103.git7f2769b.el7.centos.x86_64
已加载插件：fastestmirror
正在解决依赖关系
--> 正在检查事务
---> 软件包 docker-common.x86_64.2.1.13.1-103.git7f2769b.el7.centos 将被 删除
--> 解决依赖关系完成

依赖关系解决

=================================================================================================================================================================================
 Package                                 架构                             版本                                                           源                                 大小
=================================================================================================================================================================================
正在删除:
 docker-common                           x86_64                           2:1.13.1-103.git7f2769b.el7.centos                             @extras                           4.4 k

事务概要
=================================================================================================================================================================================
移除  1 软件包

安装大小：4.4 k
是否继续？[y/N]：y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在删除    : 2:docker-common-1.13.1-103.git7f2769b.el7.centos.x86_64                                                                                                      1/1 
  验证中      : 2:docker-common-1.13.1-103.git7f2769b.el7.centos.x86_64                                                                                                      1/1 

删除:
  docker-common.x86_64 2:1.13.1-103.git7f2769b.el7.centos                                                                                                                        

完毕！
[root@dev002 ~]# docker
-bash: /usr/bin/docker: 没有那个文件或目录
[root@dev002 ~]# docker -v
-bash: /usr/bin/docker: 没有那个文件或目录
[root@dev002 ~]# curl -fsSL https://get.docker.com/ | sh
# Executing docker install script, commit: 93d2499759296ac1f9c510605fef85052a2c32be
+ sh -c 'yum install -y -q yum-utils'
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
+ sh -c 'yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo'
已加载插件：fastestmirror
adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
grabbing file https://download.docker.com/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
repo saved to /etc/yum.repos.d/docker-ce.repo
+ '[' stable '!=' stable ']'
+ sh -c 'yum makecache'
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
epel/x86_64/metalink                                                                                                                                      | 7.0 kB  00:00:00     
 * base: mirrors.aliyun.com
 * epel: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
base                                                                                                                                                      | 3.6 kB  00:00:00     
docker-ce-stable                                                                                                                                          | 3.5 kB  00:00:00     
extras                                                                                                                                                    | 2.9 kB  00:00:00     
mysql-connectors-community                                                                                                                                | 2.6 kB  00:00:00     
mysql-tools-community                                                                                                                                     | 2.6 kB  00:00:00     
mysql57-community                                                                                                                                         | 2.6 kB  00:00:00     
updates                                                                                                                                                   | 2.9 kB  00:00:00     
(1/19): docker-ce-stable/7/x86_64/updateinfo                                                                                                              |   55 B  00:00:00     
(2/19): base/7/x86_64/other_db                                                                                                                            | 2.6 MB  00:00:00     
(3/19): docker-ce-stable/7/x86_64/primary_db                                                                                                              |  70 kB  00:00:00     
(4/19): docker-ce-stable/7/x86_64/other_db                                                                                                                | 122 kB  00:00:00     
(5/19): docker-ce-stable/7/x86_64/filelists_db                                                                                                            |  29 kB  00:00:02     
(6/19): epel/x86_64/filelists_db                                                                                                                          |  12 MB  00:00:01     
(7/19): epel/x86_64/prestodelta                                                                                                                           | 1.2 kB  00:00:00     
(8/19): extras/7/x86_64/filelists_db                                                                                                                      | 259 kB  00:00:00     
(9/19): extras/7/x86_64/other_db                                                                                                                          | 145 kB  00:00:00     
(10/19): epel/x86_64/other_db                                                                                                                             | 3.4 MB  00:00:00     
(11/19): base/7/x86_64/filelists_db                                                                                                                       | 7.2 MB  00:00:02     
(12/19): mysql-connectors-community/x86_64/other_db                                                                                                       |  25 kB  00:00:00     
(13/19): mysql-tools-community/x86_64/other_db                                                                                                            |  21 kB  00:00:00     
(14/19): mysql-connectors-community/x86_64/filelists_db                                                                                                   | 127 kB  00:00:01     
(15/19): updates/7/x86_64/other_db                                                                                                                        | 960 kB  00:00:00     
(16/19): mysql57-community/x86_64/other_db                                                                                                                |  81 kB  00:00:01     
(17/19): updates/7/x86_64/filelists_db                                                                                                                    | 7.4 MB  00:00:01     
(18/19): mysql-tools-community/x86_64/filelists_db                                                                                                        | 447 kB  00:00:02     
(19/19): mysql57-community/x86_64/filelists_db                                                                                                            | 1.7 MB  00:00:02     
元数据缓存已建立
+ '[' -n '' ']'
+ sh -c 'yum install -y -q docker-ce'
警告：/var/cache/yum/x86_64/7/docker-ce-stable/packages/docker-ce-20.10.12-3.el7.x86_64.rpm: 头V4 RSA/SHA512 Signature, 密钥 ID 621e9f35: NOKEY
docker-ce-20.10.12-3.el7.x86_64.rpm 的公钥尚未安装
导入 GPG key 0x621E9F35:
 用户ID     : "Docker Release (CE rpm) <docker@docker.com>"
 指纹       : 060a 61c5 1b55 8a7f 742b 77aa c52f eb6b 621e 9f35
 来自       : https://download.docker.com/linux/centos/gpg
+ version_gte 20.10
+ '[' -z '' ']'
+ return 0
+ sh -c 'yum install -y -q docker-ce-rootless-extras'
软件包 docker-ce-rootless-extras-20.10.12-3.el7.x86_64 已安装并且是最新版本

================================================================================

To run Docker as a non-privileged user, consider setting up the
Docker daemon in rootless mode for your user:

    dockerd-rootless-setuptool.sh install

Visit https://docs.docker.com/go/rootless/ to learn about rootless mode.


To run the Docker daemon as a fully privileged service, but granting non-root
users access, refer to https://docs.docker.com/go/daemon-access/

WARNING: Access to the remote API on a privileged Docker daemon is equivalent
         to root access on the host. Refer to the 'Docker daemon attack surface'
         documentation for details: https://docs.docker.com/go/attack-surface/

================================================================================

[root@dev002 ~]# systemctl restart docker
[root@dev002 ~]# ls
config  Dockerfile  target
[root@dev002 ~]# docker version
Client: Docker Engine - Community
 Version:           20.10.12
 API version:       1.41
 Go version:        go1.16.12
 Git commit:        e91ed57
 Built:             Mon Dec 13 11:45:41 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.12
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.12
  Git commit:       459d0df
  Built:            Mon Dec 13 11:44:05 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.12
  GitCommit:        7b11cfaabd73bb80907dd23182b9347b4245eb5d
 runc:
  Version:          1.0.2
  GitCommit:        v1.0.2-0-g52b36a2
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
[root@dev002 ~]# docker info
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Docker Buildx (Docker Inc., v0.7.1-docker)
  scan: Docker Scan (Docker Inc., v0.12.0)

Server:
 Containers: 22
  Running: 0
  Paused: 0
  Stopped: 22
 Images: 52
 Server Version: 20.10.12
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7b11cfaabd73bb80907dd23182b9347b4245eb5d
 runc version: v1.0.2-0-g52b36a2
 init version: de40ad0
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 3.10.0-1062.4.1.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 4
 Total Memory: 5.669GiB
 Name: dev002
 ID: BRX7:5AER:ACGK:TYDF:WK27:5MPS:WXYI:IORP:55CY:PDRV:EZ2X:AMLA
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false

WARNING: overlay2: the backing xfs filesystem is formatted without d_type support, which leads to incorrect behavior.
         Reformat the filesystem with ftype=1 to enable d_type support.
         Running without d_type support will not be supported in future releases.
[root@dev002 ~]# docker images
REPOSITORY                            TAG       IMAGE ID       CREATED         SIZE
sgs_anhui/~                1.0.0     93fa676dd55b   47 hours ago    751MB
sgs_anhui/ability-checkin             1.0.0     19322f1d9e6b   2 days ago      735MB
mylogstash                            7.3.0     0172429f71ed   23 months ago   840MB
app                                   ubuntu    94fe3aa5b8de   2 years ago     957MB
mariadb                               latest    e93d99aa9076   2 years ago     355MB
app                                   docker    f874300f6ac0   2 years ago     681MB
ubuntu                                18.04     775349758637   2 years ago     64.2MB
docker.elastic.co/logstash/logstash   7.3.0     213acaa482ef   2 years ago     840MB
logstash                              7.3.0     213acaa482ef   2 years ago     840MB
kibana                                7.3.0     8bcee4a4f79d   2 years ago     989MB
elasticsearch                         7.3.0     bdaab402b220   2 years ago     806MB
java                                  8         d23bdf5b1b1b   5 years ago     643MB
[root@dev002 ~]# 
```

docker build -t sgs_anhui/ability-checkin:1.0.0 .

