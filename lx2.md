# 第二周
## 第一天
### 小系统U盘批量制作
```bash
sh write_to_flash.sh minios.4.9.0.img //指定镜像路径；
watch 'ps -ef | grep minios' //查看镜像的写录的进程；
sh flash_adjust.sh  //调整U盘的文件
[root@localhost ~]# ls
0_write_to_disk.sh(需要注释里面的内容)  flash_adjust.sh   write_to_flash.sh
minios.4.9.0.img  set_ip.sh  //以上文件必须存在；
```
```bash
write_to_flash.sh的内容：
#!/bin/sh
num=$(lsscsi -s|grep 8.00GB|wc -l)
i=1
while [ $i -le $num ]
do	disk=$(lsscsi -s|grep 8.00GB|awk '{print $7}'|sed -n $i\p)
 	nohup dd if=$1 of=$disk bs=3M  > /dev/null 2>&1 & 
i=$(($i+1))
done
```
```bash
flash_adjust.sh的内容：
#!/bin/sh
i=1
num=$(lsscsi -s|grep 8.00GB|awk '{print $7}'|wc -l)
while [ $i -le $num ]
	do mkdir -p /disk/usb$i
           usb=$(lsscsi -s|grep 8.00GB|awk '{print $7}'|sed -n $i\p)
           mount $usb'1' /disk/usb$i
	   cat /root/0_write_to_disk.sh > /disk/usb$i/root/0_write_to_disk.sh 
	   #cat /root/network.info > /disk/usb$i/network.info
	   cp /root/set_ip.sh /disk/usb$i/root/
	   echo /dev/sdb > /disk/usb$i/root/.ssd
	   echo /dev/sdf > /disk/usb$i/tmp/.install_to_disk
	   umount $usb'1'
	   rm -rf /disk/usb$i
	   i=$(($i+1))
	done	
```
### 硬盘的更换
```bash
1. 停服务，取消挂载硬盘
systemctl stop skypiea
umount /disk/sata12)
2. 执行 disk_read.sh 
脚本#!/bin/shTIMES=100for i 
in `seq 1 $TIMES`;
do        
echo $i        
for dev in `df -h|grep /disk/sata|awk '{print $1}'`;do
                dev=${dev%1}                
nohup dd if=$dev of=/dev/null bs=10M count=50 iflag=direct 1>&2>/dev/null &        done        
sleep 10
done
观察服务器前面板 找到读写指示灯不闪烁的硬盘
3. 服务器端查看 /var/log/messages文件 确定新盘的盘符，执行replace.sh，输入盘符以及标签
#!/bin/sh
read -p "input drive label : " DRIVE LABEL
parted -s $DRIVE mklabel gpt && parted -s $DRIVE mkpart primary 0% 100%
mkfs.ext4 -L $LABEL -T largefile $DRIVE'1'
grep -w "$LABEL" /etc/rc.local > /tmp/tmp.sh
sh /tmp/tmp.sh
rm -rf /tmp/tmp.sh4）
df -h查看硬盘是否挂载成功,恢复服务 systemctl start skypiea
```