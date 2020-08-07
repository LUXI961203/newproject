# 第四周
## 第一天
### 远程访问的步骤
```bash
[root@ops .ssh]# ll id_rsa  //查看软连接；
lrwxrwxrwx. 1 root root 4 Jul 23 08:54 id_rsa -> x.lu
[root@ops .ssh]# rm -fr id_rsa  //将原有的软连接删除；
[root@ops .ssh]# rm -fr x.lu   //移除x.lu的私钥；
[root@ops .ssh]# rz  //上传远程x.lu主机的私钥；
[root@ops .ssh]# chmod 400 id_rsa_2048 //修改文件的权限；
[root@ops .ssh]# ssh -i id_rsa_2048 x.lu@115.231.100.76 -p 65422  //-i identity_file 从-i id_rsa_2048中读取传输时使用的密钥文件，此参数直接传递给ssh，然使用65422端口；
```
### SCP的运用
```bash
scp的六种使用方法:
1. 本地复制远程文件：（把远程的文件复制到本地）
scp root@www.test.com:/val/test/test.tar.gz /val/test/test.tar.gz
2. 远程复制本地文件：（把本地的文件复制到远程主机上）
scp /val/test.tar.gz root@www.test.com:/val/test.tar.gz
3. 本地复制远程目录：（把远程的目录复制到本地）
scp -r root@www.test.com:/val/test/ /val/test/
4. 远程复制本地目录：（把本地的目录复制到远程主机上）
scp -r ./ubuntu_env/ root@192.168.0.111:/home/pipi
pika:/media/pika/files/machine_learning/datasets$scp -r SocialNetworks/ piting@192.168.0.172:/media/data/pipi/datasets
5. 本地复制远程文件到指定目录：（把远程的文件复制到本地）
scp root@www.test.com:/val/test/test.tar.gz /val/test/
6. 远程复制本地文件到指定目录：（把本地的文件复制到远程主机上）
scp /val/test.tar.gz root@www.test.com:/val/
```
## 第二天
### 将镜像写入U盘
```bash
1. 下载镜像到指定文件夹，然后执行下列命令；
2. dd if=/root/centos6.8.iso of=/dev/sdb
```
### 装机
```bash
1. centos.6的装机
第一步：进入U盘系统；
第二步：划分一个40000MB根（/）分区和一个8000MBswap分区；
第三步：重启；
2.centos.7的装机
第一步：进入U盘系统；
第二步：划分一个40G的根（/）分区和一个8Gswap分区；
第三步：配置开机密码；
```
## 第三天
### 磁盘阵列
RAID0：RAID 0 技术把多块物理硬盘设备（至少两块）通过硬件或软件的方式串联在一起，组成一个大的卷组，并将数据依次写入到各个物理硬盘中。这样一来，在理想的状态下，硬盘 设备的读写性能会提升数倍，但是若任意一块硬盘发生故障将导致整个系统的数据都受到破坏。
优点：条带化机制，一次性读两条，读取速度快；缺点：一块损坏，另一块也没用了。

RAID5：RAID5 技术是把硬盘设备的数据奇偶校验信息保存到其他硬盘设备中。 RAID 5 磁盘阵列组中数据的奇偶校验信息并不是单独保存到某一块硬盘设备中，而是存储到 除自身以外的其他每一块硬盘设备上，这样的好处是其中任何一设备损坏后不至于出现致命缺陷。
优点：通过校验值进行恢复；缺点：数据恢复时会进行降级处理，当恢复海量数据时可能会丢失数据；

RAID10：RAID 10 技术需要至少 4 块硬盘来组建，其中先分别两两制作成 RAID 1 磁盘阵列，以保 证数据的安全性；然后再对两个 RAID 1 磁盘阵列实施 RAID 0 技术，进一步提高硬盘设 备的读写速度。这样从理论上来讲，只要坏的不是同一组中的所有硬盘，那么多可以损 坏 50%的硬盘设备而不丢失数据。由于RAID 10技术继承了RAID 0的高读写速度和RAID 1 的数据安全性，在不考虑成本的情况下 RAID 10 的性能都超过了 RAID 5，因此当前成为广泛使用的一种存储技术。
优点：底层可以坏不同阵列的两块盘，注意两块盘不能坏在同一边；

## 第四天
### 惠普服务器读取不到硬盘解决方法：
1. 将光卡禁用后，重启服务器；
2. 然后将服务器改为直通类型，再次重启；

## 第五天
1. 磁盘阵列的创建
```bash
fdisk /dev/sda
n->t->fd
[root@teacher Desktop]# partprobe /dev/sda

[root@teacher Desktop]# mdadm -C /dev/md0 -l5 -n3 -x1 /dev/sda[5-8]     
-C:创建   md代表做出来的阵列盘符,0代表第一个盘符   -l代表级别  -n运行 -x备份，如果没有备份就不写

[root@teacher Desktop]# mdadm -D /dev/md0                 #查看详细信息
[root@teacher Desktop]# watch -n1 mdadm -D /dev/md0       #一秒看一次

[root@teacher Desktop]# mkfs.ext4 /dev/md0                #格式化
[root@teacher Desktop]# mkdir /mnt/md0              
[root@teacher Desktop]# mount /dev/md0 /mnt/md0/          #临时挂载
[root@teacher Desktop]# cp /etc/*.conf /mnt/md0/

[root@teacher Desktop]# mdadm /dev/md0 -f /dev/sda7       #损坏一块硬盘
mdadm: set /dev/sda7 faulty in /dev/md0

[root@teacher Desktop]# mdadm /dev/md0 -r /dev/sda7       #移除
mdadm: hot removed /dev/sda7 from /dev/md0
 
[root@teacher Desktop]# mkfs.ext4 /dev/sda7
[root@teacher Desktop]# mdadm /dev/md0 -a /dev/sda7       #添加
mdadm: added /dev/sda7
[root@teacher Desktop]# mdadm -D /dev/md0

[root@teacher Desktop]# umount /mnt/md0/
[root@teacher Desktop]# mkfs.ext4 /dev/md0                #重新格式化
```