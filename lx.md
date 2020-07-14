#第一天
##小型服务器安装
dd if=/dev/zero of=/dev/sda bs=4k count=512 
              sh set.ip
              echo /dev/sdb /root/.ssd
              echo /dev/sdf /tmp/install
              vi 以0开头的脚本
              sh 以0开头的脚本
#第二天
##Python在Linux系统上的安装
Linux中python的安装：
1.安装编译相关工具
yum -y groupinstall "Development tools"
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
yum install libffi-devel -y

2.下载安装包解压
cd #回到用户目录
wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz
tar -xvJf  Python-3.7.0.tar.xz

3.编译安装python
mkdir /usr/local/python3 #创建编译安装目录
cd Python-3.7.0
./configure --prefix=/usr/local/python3
make && make install

4.创建软连接
ln -s /usr/local/python3/bin/python3 /usr/local/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3

5.验证是否成功
python3 -V
pip3 -V
##新接触的Linux命令
补全安装包：yum install -y bash-completion