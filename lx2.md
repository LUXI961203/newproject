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
        break
```
## 第三天
### Python中字符串的方法
1. Python头格式：
```py
     #!/usr/bin/python
    -*- coding: UTF-8 -*-
```
2. find()方法：find()方法用于检测字符串中是否包含子字符串str;
```py
find()用法： str.find(str, beg=0, end=len(string));
str -- 指定检索的字符串
beg -- 开始索引，默认为0。
end -- 结束索引，默认为字符串的长度。
```
3. Python join() 方法用于将序列中的元素以指定的字符连接生成一个新的字符串。
```py
join()用法：
str.join(sequence)；
sequence -- 要连接的元素序列；
```
## 第四天
### Python关键词
```py
1. and：表示逻辑"与";
2. del：用于list列表操作，删除一个或者连续几个元素;
3. from：导入相应的模块，用import或者from...import;
4. not：表示逻辑‘非’;
5. while：while循环，允许重复执行一块语句，一般无限循环的情况下用它;
6. as：当作，as单独没有意思，是这样使用：with....as用来代替传统的try...finally语法的;
7.elif: 和if配合使用的，if语句中的一个分支用elif表示；
8.global: 定义全局变量，我的理解就是：要想给全局变量重新赋值，就要global一下全局变量；
9.or：表示逻辑“或”；
10.with：和as一起用，使用的方法请看as，在上面；
11.assert：表示断言（断言一个条件就是真的，如果断言出错则抛出异常）用于声明某个条件为真，如果该条件不是真的，抛出异常：AssertionError；
12.else：看下面if的解释；
13.if：if语句用于选择分支，依据条件选择执行那个语句块；
14.pass：pass的意思就是什么都不做。用途及理解：当我们写一个软件的框架的时候，具体方法啊，类啊之类的都不写，等着后续工作在做；
15.yield：迭代，用起来和return很像，但它返回的是一个生成器；
16.break  ：作用是终止循环，程序走到break的地方就是循环结束的时候。（有点强行终止的意思）注意：如果从for或while循环中终止（break）之后 ，else语句不执行；
17.except ：和try一起使用，用来捕获异常；
18.import ：用来导入模块，有时这样用from....import；
19.print：输出；
20.class：定义类；
21.continue：Python continue 语句跳出本次循环，而break跳出整个循环，continue 语句用来告诉Python跳过当前循环的剩余语句，然后继续进行下一轮循环，continue语句用在while和for循环中；
22.raise: raise是用来抛出异常的，一旦抛出异常后，后续的代码将无法运行;
23.try：try 关键字用于 try...except 块中。它定义了代码测试块是否包含任何错误；
24.finally：最后，finally 关键字在 try ... except 块中使用。它定义了一个代码块，当 try...except...else 块结束时，该代码块将运行，无论 try 块是否引发错误，都将执行 finally 块；
25.None: None 关键字用于定义 null 值，或根本没有值。None 与 0、False 或空字符串不同。 None 是其自身的数据类型（NoneType），并且只有 None 可以是 None;
26.def: def 关键字用于创建（或定义）函数;
27.True: 关键字是一个布尔值，是比较运算的结果,True 关键字与 1 相同（Fals 与 0 相同）;
28.False: False 关键字是布尔值，是比较运算的结果。False 关键字等同于 0（True 等同于 1）;
29.exec: exec 执行储存在字符串或文件中的Python语句，相比于 eval，exec可以执行更复杂的 Python 代码;
30.lambda: lambda 关键字用于创建小型匿名函数。Lambda 函数可以接受任意数量的参数，但只能拥有一个表达式。这个表达式会被计算并返回结果;
