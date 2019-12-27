＃otdr安装准备
1、yum update

2、mariaDB. 安装
https://downloads.mariadb.org/mariadb/repositories/distro=CentOSdistro_release=centos7-amd64--centos7mirror=liquidtelecomversion=.

3、gcc 安装 （版过低，提示“CXXABI_.3.8”） 
必备组件安装
yum install -y gcc gcc-c++ bzip2
root用户执行，用户目录。其实cd哪里都阔以。
cd ~/
下载gcc源代码
wget https://ftp.gnu.org/gnu/gcc/gcc-7.4./gcc-7.4..tar.gz
wget -c http://mirror.linux-ia64.org/gnu/gcc/releases/gcc-7.4./gcc-7.4..tar.xz
解压
tar -zxvf gcc-7.4..tar.gz
源代码目录
cd gcc-7.4.
下载一些必须的东西
./contrib/download\_prerequisites
如果下载不下来，或者下载缓慢可以考虑查命令行拿下载地址自己down下拉后，放源代码目录。具体地址：ftp://gcc.gnu.org/pub/gcc/infrastructure/，需要下载的几个源代码包如下，可以查./contrib/download\_prerequisites文件。
gmp='gmp-6...tar.bz2'
mpfr='mpfr-3..4.tar.bz2'
mpc='mpc-..3.tar.gz'
isl='isl-.6..tar.bz2'
接着创建一个目录，用于gcc build
mkdir gcc-build-7.4.
cdbuild目录，准备开始编译了。
cd gcc-build-7.4.
编译的config，disable-multilib 64位编译标记。具体可查官方文档
../configure -enable-checking=release -enable-languages=c,c++ -disable-multilib
接着就是漫长的编译等待了
make 不知是否可以使用make -j8之类的开启多核编译是否会快一点，我反正是等了好几个小时
next
make install
重建立软连接
find / -name "libstdc++.so*" 找自己的文件路径
把libstdc++.socopy/usr/lib64目录，类似下面的命令
cp /root/gcc-7.4./gcc-build-7.4./x86\_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6..24 /usr/lib64
cd /usr/lib64
 rm -rf libstdc++.so.6 删除原来的
 ln -s libstdc++.so.6..24 libstdc++.so.6 重建立软连
 gcc -v 输出，是不是如下图变成7.4.拉。

4、gt5.. 安装

5、openssl 版过低，提示“OPENSSL_..”


6、


7、
验证gcc版是否满足要求
strings /lib64/libstdc++.so.6 |grep CXXABI_
验证OPENSSL版是否满足要求
strings /lib64/libssl.so... | grep OPENSSL_


scp newcable@2.68..42:/usr/lib/x86_64-linux-gnu/libssl.so... ./

scp -P 244 ./libssl.so... root@c248h84.iok.la:/usr/lib64/

ssh -p 244 root@c248h84.iok.la

1、gcc 版过低，提示“CXXABI_.3.8”
2、openssl 版过低，提示“OPENSSL_..”
3、qt5..安装

sudo mv /usr/bin/gcc /usr/bin/gcc4
sudo mv /usr/bin/c++ /usr/bin/c++4

sudo ln -s /usr/local/bin/x86_64-pc-linux-gnu-gcc /usr/bin/gcc
sudo ln -s /usr/local/bin/x86_64-pc-linux-gnu-c++ /usr/bin/c++


oracle 
export ORACLE_HOME=/usr/lib/oracle/2./client64
export TNS_ADMIN=$ORACLE_HOME/network/admin
export NLS_LANG= 'simplifiedchinese_china.UTF8'
export LD_LIBRARY_PATH=$ORACLE_HOME/lib 
export PATH=$ORACLE_HOME/bin:$PATH


openssl

wget https://www.openssl.org/source/openssl-..d.tar.gz
tar zxvf openssl-..d.tar.gz
make
make install
mv /usr/bin/openssl /usr/bin/openssl.bak
mv /usr/include/openssl /usr/include/openssl.bak
ln -s /usr/local/bin/openssl /usr/bin/openssl
ln -s /usr/local/include/openssl /usr/include/openssl
echo “/usr/local/lib64” >> /etc/ld.so.conf
ldconfig -v