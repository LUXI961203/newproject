# 第九周
## 第一天
### 乌班图（Ubuntu）apt的用法
```bash
apt 命令
被取代的命令说明
apt install||apt-get install
安装新包

apt remove||apt-get remove
卸载已安装的包（保留配置文件）

apt purge||apt-get purge
卸载已安装的包（删除配置文件）

apt update||apt-get update
更新软件包列表

apt upgrade||apt-get upgrade
更新所有已安装的包

apt autoremove||apt-get autoremove
卸载已不需要的包依赖

apt full-upgrade||apt-get dist-upgrade
自动处理依赖包升级

apt search||apt-cache search
查找软件包

apt show||apt-cache show
显示指定软件包的详情

apt list	列出包含条件的包（已安装，可升级等）
apt edit-sources	编辑源列表
```
## 第二天
### SHELL脚本
```bash
$0	    当前脚本的名字
$n	    传递给脚本或者函数的参数，n表示第几个参数
$#	    传递给脚本或函数的参数个数
$*	    传递给脚本或函数的所有参数
$@	    传递给脚本或者函数的所有参数
$$	    当前shell脚本进程的PID
$?	    函数返回值，或者上个命令的退出状态
$BASH	    BASH的二进制文件问的路径
$BASH_ENV	        BASH的启动文件
$BASH_VERSINFO[n]	BASH版本信息，有六个元素
$BASH_VERSION	        BASH版本号
$EDITOR 	脚本所调用的默认编辑器
$EUID   	当前有效的用户ID
$FUNCNAME	当前函数名
$GROUPS 	当前用户所属组
$HOME   	当前用户家目录
$HOSTTYPE	主机类型
$LINENO	    当前行号
$OSTYPE	    操作系统类型
$PATH	    PATH路径
$PPID	    当前shell进程的父进程ID
$PWD	    当前工作目录
$SECONDS    当前脚本运行秒数
$TMOUT	    不为0时，超过指定的秒将退出shell
$UID	    当前用户ID
```