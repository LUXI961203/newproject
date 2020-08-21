# 第六周
## 第一天
### 2U服务器装系统
```bash
第一步：Ctrl+r 进入配置RAID 0 ，然后重启;
第二步：F11 进入管理系统，选择U盘启动,然后开始装系统；
1、fdisk -l | grep /dev/;
2、echo /dev/sdm > /root/.ssd;
3、echo /dev/sdn > /tmp/.install;
4、sh set.ip.sh;
5、sh 0_write;(选择yes)
6、reboot;
第三步：F2 进入设置时间和开机优先启动项，然后重启；
第四步：开机进入系统查看是否配置完毕；
1、fdisk -l | grep /dev/;
2、ip a , ip r;
3、cat /proc/cpuinfo | grep -c processor;
4、fdisk -l | grep /d;
5、free -g;
```
## 第二天
### OSPF的概述
```bash
ospf区域划分的作用：
1.减少LSA泛洪的范围，维持整个OSPF AS的稳定性，降低路由抖动的频率和范围
2.节约带宽
3.节约硬件耗损
4.路由器3个表：链路状态数据库LSDB（一个区域的拓扑图）、路由表、邻居列表，采用SPF算法计算最小成本（cost）作为最优路径。---RIP以跳数作位度量值，ospf以成本作为度量值。ospf是无环路由。不存在水平分割等等
5.邻接关系建立过程（dowm,init,2-way,exstart,exchange,loading,full7个状态的演变。依次发送了Holle，DBD，LSR,LSU(包含链路状态),LSACK）
6.四种网络类型：BMA,NBMA,点到点，点到多点。
7.DR,BDR选取：先看端口priority，再看router-id大小

ospf配置：
 router ospf 200
 network 192.200.10.4 0.0.0.3 area 0 /骨干区域；
 network 192.1.0.64 0.0.0.63 area 2 /非骨干区域；
```
## 第三天
### 上架测试
```
ansible 格式：
117.27.142.226 ansible_ssh_port=65422 ansible_ssh_user=root
117.27.142.227 ansible_ssh_port=65422 ansible_ssh_user=root
117.27.142.228 ansible_ssh_port=65422 ansible_ssh_user=root
117.27.142.229 ansible_ssh_port=65422 ansible_ssh_user=root
```
```bash
ansible -i fz all -m shell -a "ethtool bond0 | grep Speed"/查看运行速率；
ansible -i fz all -m shell -a "free -g"/查看内核数量；
ansible -i fz all -m shell -a "fdisk -l | grep -c 2000" -f10 /过滤出2000的盘符；
ansible -i fz all -m ping /测试连接；
upssh.old root@117.27.142.236 /远程登陆；
```
## 第四天
### 单臂路由
```bash
1.单臂路由：路由器沿着交换机的一侧连接多个VLAN，VLAN隔离广播域，利用单臂路由可以通信（不同VLAN主机通信）；
2.Trunk：trunk链路可以给除VLAN1的VLAN打标签（走的是打标签的以太帧）；
3.交换机主机端access，路由端trunk（不一定非是trunk链路，也可以是access链路）单臂路由是trunk链路
```
### 解决DELL2U网卡问题
```bash
1.modprobe ixgbe allow_unsupported_sfp=1
2.echo "options ixgbe allow_unsupported_sfp=1" > /etc/modprobe.d/ixgbe.conf
```