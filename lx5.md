# 第五周
## 第一天
### 热备份路由协议（HSRP）
1. HSRP的状态：
    ```bash
    初始状态->学习状态->倾听状态->发言状态->备份状态->活跃状态;
    ```
2. HSRP认证
    ```bash
    1. 通过在HRSP消息内插入共享的文明密码而实现；
    2. 防止将路由器错误地配置到其它的HSRP组内；
    3. 仅仅具有一定的安全性，明文传输；
    ```
3. HSRP配置
    ```bash
    1.配置路由器为HSRP的成员，使用接口配置命令
      standby 10 ip 192.168.10.254 #配置HSRP虚拟IP
    2.配置优先级
      standby 10 priority 150      #配置HSRP优先级
    3.配置占先权
      standby 10 preempt           #配置HSRP抢先权
## 第二天
### smartctl的用法
```bash
yum -y install smartmontools //安装smartctl的包；

smartctl -a 显示硬盘所有SMART信息。

smartctl -i 显示硬盘model number, serial number,是否开启SMART等信息。

smartctl -s on 如果没有打开SMART技术，使用该命令打开SMART技术。

smartctl -t short 后台检测硬盘，消耗时间短

smartctl -t long 后台检测硬盘，消耗时间长

smartctl -C -t short 前台检测硬盘，消耗时间短

smartctl -C -t long 前台检测硬盘，消耗时间长

smartctl -X 中断后台检测硬盘。

smartctl -l selftest 显示硬盘检测日志。

smartctl -l error 显示硬盘错误汇总。
```
### 测试磁盘的读写能力
```bash
1. 测试磁盘的写能力：
time dd if=/dev/zero of=/testw.dbf bs=4k count=100000；
2. 测试磁盘的读能力：
time dd if=/dev/sdb of=/dev/null bs=4k；
3. 测试磁盘的纯写入能力：
dd if=/dev/zero of=test bs=8k count=10000 oflag=direct；
4. 测试磁盘的纯读取能力：
dd if=test of=/dev/null bs=8k count=10000 iflag=direct；
```
## 第三天
### RIP（距离矢量路由协议）
```bash
1. RIP的工作原理:
  rip路由协议向邻居发送整个路由表信息；
  rip路由协议以跳数作为度量值根据跳数的多少来选择最佳路由；
  rip的最大跳数为15跳，16跳为不可达；
  经过一系列的路由更新，网络中的每个路由器都具有一张完整的路由表的过程，称为收敛；
2. RIPv1与RIPv2的区别：
  rip1只支持有类路由，rip2支持有类和五类路由；
  rip1不支持VLSM/CIDR，rip2支持；
  rip1不支持连续的子网划分，rip2支持；
```



