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
num=$(lsscsi -s | grep 8.00GB | wc -l)
i=1
while [ $i -le $num ]
do	disk=$(lsscsi -s | grep 8.00GB | awk '{print $7}' | sed -n $i\p)
 	nohup dd if=$1 of=$disk bs=3M  > /dev/null 2>&1 & 
i=$(($i+1))
done
```
```bash
flash_adjust.sh的内容：
#!/bin/sh
i=1
num=$(lsscsi -s | grep 8.00GB | awk '{print $7}' | wc -l)
while [ $i -le $num ]
	do mkdir -p /disk/usb$i
           usb=$(lsscsi -s | grep 8.00GB | awk '{print $7}' | sed -n $i\p)
           mount $usb'1' /disk/usb$i
	   cat /root/0_write_to_disk.sh > /disk/usb$i/root/0_write_to_disk.sh 
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
脚本#!/bin/sh
TIMES=100
for i 
in `seq 1 $TIMES`;
do        
echo $i        
for dev in `df -h | grep /disk/sata | awk '{print $1}'`;
do
                dev=${dev%1}                
nohup dd if=$dev of=/dev/null bs=10M count=50 iflag=direct 1>&2>/dev/null &
done        
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
## 第二天
### Python猜数字
```py
#coding:utf-8
#from random import *
import random as rand  //给导入模块取别名；

def ask (s,n1,n2):
    while True:
        try :
            num = int(input(s))
            if num in range (n1,n2):
                return num
            else:
                print("犯规")
        except :
            print("犯规")
//读取生命值，并做检查，合法则跳出死循环；
//def 函数的定义，使用及参数传递的说明；

ming = ask("请输入你有几条命：",1,20)
fanwei = ask("请输入一个范围：",20,300)
suiji = rand.randint(1,fanwei)

while True:
    if ming == 0:
        print("你死了！,数字是%s" %suiji)
        break
    shuzi = int(input("输入一个数字："))
    if shuzi < suiji:
        print ("小了")
        ming -= 1
    elif shuzi > suiji:
        print ("大了")
        ming -= 1
    else:
        print ("猜对了！")
        brea
```
## 第三天
### Python中字符串的方法
1. find()方法：find()方法用于检测字符串中是否包含子字符串str;
```py
find()用法： str.find(str, beg=0, end=len(string));
str -- 指定检索的字符串
beg -- 开始索引，默认为0。
end -- 结束索引，默认为字符串的长度。
```
1. Python join() 方法用于将序列中的元素以指定的字符连接生成一个新的字符串。
```py
join()用法：
str.join(sequence)
