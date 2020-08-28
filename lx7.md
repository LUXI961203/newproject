# 第七周
## 第二天
### Shell脚本
```bash
1. 头格式：
#！ /bin/bash；

2.执行或调用脚本：
sh+脚本，chomd+x 脚本 ，bash 脚本；

3.脚本中的符号表示：
$#:汇总脚本的个数；
$0:输出运行脚本的名字；
$1:输出脚本运行的第一个值；
$2:输出脚本运行的第二个值；
$@ $*:输出脚本的统计结果；
$$:输出脚本的进程号；

4.shell的语法：
if条件判断语句：
单分支语句：语法（if+[ 条件 ] then 语句1 语句2 fi）

双分支语句：
if+[ 条件 ]
then
语句1
else
语句2
fi

多分支语句：
if+[ 条件 ]
then
语句1
elif+条件
then
语句2
elif+条件
then
语句3
fi（结束）

case语句：
case语句：条件判断；（比if简洁）
语法：
case 变量名 in
模式1 ）
命令序列;;
模式2）
命令序列;;

while循环语句：
语法：
while+条件
do
语句
done（结束）<(将值往回倒);
```
## 第三天
### SAMBA服务
```bash
实现不同操作系统之间的共享；
NAS服务：network area service；
smb端口号：137或138
nmbd：Windows的进程；端口号：139或445；

smb的工作流程：
client------------------server
1.negitiate protocol 磋商协议
------------negprot----->
<-----------negprot------
2.session connection 会话连接
------session setup----->
<-----session setup------
3.access share source 访问共享资源
------tree connect------>
<-----tree connect------
4.disconnect 断开连接
------tree disconnect-->
<-----tree disconnect---

smb主配置文件：
[共享名]
comment = 描述信息
path = 共享文件夹的路径，必须是绝对路径；
public = 文件是否公共访问，yes或no；
writable = 文件的是否可写，yes或no，yes允许客户端写入内容；
printable = 文件打印，no；
write list = 允许对文件访问的用户列表，与public有冲突；
browseable = 可浏览，no表示将共享文件隐藏，客户端不能看到；
host allow = 指定网段访问共享文件，不能写域名，不写默认所有；
```

## 第四天
### fping
```bash
1.fping的介绍：Fping类似于ping，但比ping强大。Fping与ping不同的地方在于，fping可以在命令行中指定要ping的主机数量范围，也可以指定含有要ping的主机列表文件。与ping要等待某一主机连接超时或发回反馈信息不同，fping给一个主机发送完数据包后，马上给下一个主机发送数据包，实现多主机同时ping。如果某一主机ping通，则此主机将被打上标记，并从等待列表中移除，如果没ping通，说明主机无法到达，主机仍然留在等待列表中，等待后续操作。
2.fping的使用：
-a：显示可ping通的目标;
-A：将目标以ip地址的形式显示;
-b:ping 数据包的大小。（默认为56）;
-c:ping每个目标的次数 (默认为1);
-g:通过指定开始和结束地址来生成目标列表（例如：./fping –g 192.168.1.0 192.168.1.255）或者一个IP/掩码形式（例如：./fping –g 192.168.1.0/24）;
-l:循环发送ping;
```
## 第五天
### LVS原理（Linux Virtual Server，Linux虚拟服务器）
```bash
1.特点：可伸缩网络服务的几种结构，它们都需要一个前端的负载调度器（或者多个进行主从备份）。我们先分析实现虚拟网络服务的主要技术，指出IP负载均衡技术是在负载调度器的实现技术中效率最高的。在已有的IP负载均衡技术中，主要有通过网络地址转换（Network Address Translation）将一组服务器构成一个高性能的、高可用的虚拟服务器，我们称之为VS/NAT技术（Virtual Server via Network Address Translation）。在分析VS/NAT的缺点和网络服务的非对称性的基础上，我们提出了通过IP隧道实现虚拟服务器的方法VS/TUN （Virtual Server via IP Tunneling），和通过直接路由实现虚拟服务器的方法VS/DR（Virtual Server via Direct Routing），它们可以极大地提高系统的伸缩性。VS/NAT、VS/TUN和VS/DR技术是LVS集群中实现的三种IP负载均衡技术。
2.IP地址分为以下三种：
Director Virtual IP:调度器用于与客户端通信的IP地址，简称为VIP
Director IP:调度器用于与RealServer通信的IP地址，简称为DIP。
Real Server : 后端主机的用于与调度器通信的IP地址，简称为RIP。
```