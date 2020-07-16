# 第一天
## 小型服务器安装
Linux dd命令用于读取、转换并输出数据。
dd可从标准输入或文件中读取数据，根据指定的格式来转换数据，再输出到文件、设备或标准输出。
if=文件名：输入文件名，默认为标准输入。即指定源文件。
of=文件名：输出文件名，默认为标准输出。即指定目的文件。
bs=bytes：同时设置读入/输出的块大小为bytes个字节。
count=blocks：仅拷贝blocks个块，块大小等于ibs指定的字节数。

dd if=/dev/zero of=/dev/sda bs=4k count=512 
              sh set.ip
              echo /dev/sdb /root/.ssd
              echo /dev/sdf /tmp/install
              vi 以0开头的脚本
              sh 以0开头的脚本
# 第二天
## Python在Linux系统上的安装
Linux中python的安装：
1. 安装编译相关工具
```bash
yum -y groupinstall "Development tools"
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
yum install libffi-devel -y
```

2. 下载安装包解压
```bash
cd #回到用户目录
wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz
tar -xvJf  Python-3.7.0.tar.xz
```

3. 编译安装python
```bash
mkdir /usr/local/python3 #创建编译安装目录
cd Python-3.7.0
./configure --prefix=/usr/local/python3
make && make install
```
4. 创建软连接
```bash
ln -s /usr/local/python3/bin/python3 /usr/local/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3
```


5. 验证是否成功
```bash
python3 -V
pip3 -V
```
## 新接触的Linux命令
补全安装包：yum install -y bash-completion






## Markdown命令
笔记上传

```bash
 git status  （查看状态）
 git add .  （增加新的文件，在当前路径）
 git commit -m "18:16" （上传后看到的标识）
 git push （上传）
 ls  
 rm -fr kkk.md
 git add .
 git commit -m "18:17"
 git push
 history
```

# 第三天
## 关于Linux中SED的命令
Linux sed 命令是利用脚本来处理文本文件。

sed 可依照脚本的指令来处理、编辑文本文件。

Sed 主要用来自动编辑一个或多个文件、简化对文件的反复操作、编写转换程序等。
## 参数说明
```bash
-e：可多条命令同时执行；
 例如：sed -i -e '1a\newline' -e '2i hello' /tmp/net;

a:新增，a后面跟字符串，这些字符串在当前行的下一行出现（新增一行）；
例如： sed -i '1a\hello word' /tmp/net （在第一行后插入hello word）

i:插入，i的后面跟字符，这些字符串在当前行的上一行出现（新增一行）
例如： sed -i '3i\nmtui' /tmp/net（在第三行的前面插入nmtui）

c：取代，c后面可以跟字符串，这些字符串可以取代规定行数之间的字符串；
例如： sed -i '2,4c gogogo' /tmp/net （将2到4行用gogogo取代）

d：删除，d后面不能跟字符串；
例如：sed -i '2,4d' /tmp/net（将2到4行删除）

p：打印，选择需要的数据一行都打印出来，通常p参数通常与sed -n一起使用；
例如：cat /tmp/net | sed -n '/eth0/p'（将含有eth0的行都打印出来）

s：取代，能确定取代掉指定的参数；
例如：sed -i 's/eth0/eth1/g' /tmp/net（用eth1取代eth0）
```
# 第四天
## Python安装的脚本
```bash
#! /bin/bash
1. 安装编译相关工具
yum -y groupinstall "Development tools"
if [ $? = 0 ];then
    yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel wget
else 
yum --disablerepo='epel' groupinstall "Development Tools"
fi
2. 下载安装包解压
cd /tmp/
wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz
if [ $? = 0 ];then
	tar -xvJf  Python-3.7.0.tar.xz
else
	echo "step2 failed"
	exit 1
fi
3. 编译安装python
mkdir /usr/local/python3  //创建编译安装目录
cd Python-3.7.0
./configure --prefix=/usr/local/python3
make && make install
if [ $? = 0 ];then
	echo "install finished"
fi
4. 创建软连接
ln -s /usr/local/python3/bin/python3 /usr/local/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3
if [ $? = 0 ];then
	echo "install successful"
fi
5. 验证是否成功
python3 -V
pip3 -V
```

## 安装各项服务包的脚本
```bash
for srv in samba mariadb httpd targetcli nmap;
do
	echo $srv
	rpm -qa $srv|grep -iq $srv
	if [ $? = 0 ];then
		echo 'installed'
	else
		yum install -y $srv*
		if [ $? = 0 ];then
			systemctl enable $srv.service
			systemctl start $srv.service
		fi
	fi
done
exit 1
```
