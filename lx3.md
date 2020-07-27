# 第三周
## 第一天
### Ansible的安装和基础知识
```bash
Ansible的优点：
1. 仅需管理主机安装并配置即可，所有的配置工作都通过ssh连接下发至被管理主机。
轻量化
2. Python编写，源代码可读性强。同时playbook使用yml配置，容易上手
对于集群和ECS的支持非常完美。
3. 支持大量API接入，并且对于python的扩展支持良好。
4. 由于架构出色，理论上在管理主机性能到位的情况下可以同时配置近乎无限的主机。
```
1. Ansible的安装
    ```bash
    yum -y install epel-release //安装epel源，来获得更高版本的ansible;
    yum -y install ansible //安装ansible;
    ansible --version  //查看ansible的版本;
    ```
2. 密钥的配置
     ```bash
     将管理端的公钥加入到被管理端的.ssh/authorized_keys中;
     ```
3. 将受管理的机器IP加入管理Ansible
    ```bash
    例如：
    vim /etc/ansible/hosts
    172.26.0.81              ##不分组直接声明IP
    172.26.0.82:54208        ##申明特殊的ssh端口

    [fangwei-test]           ##申明了受管机的列表和分类名
    172.26.0.83              ##直接声明IP
    Printer     ansible_ssh_host=172.26.0.84    ##定义别名和IP
    EMR-master  ansible_ssh_host=172.26.0.85
    Snipe-it    ansible_ssh_host=172.26.0.86
    Kubernetes  ansible_ssh_host=172.26.0.87
    ```
4. 模块
    ```bash
   1. Command模块
    Command模块自如其名，就是一个很纯粹的命令执行模块，用于在受管机上进行命令的执行，其本身也可以配合很多二级参数和指令进行功能的扩展。不支持管道功能。
     用法：ansible fangwei（定义的组） -m command -a 'hostname' ;

   2. Ping模块
    ping这个模块主要用来测试能否连通目标服务器。
    用法： ansible test（定义的组） -m ping ;

   3. Copy模块
    Copy模块主要用于复制文件到远程主机；

    参数使用：
    backup：在覆盖之前将源文件备份，备份文件包含时间信息，选项：yes|no
    content：用于替代”src”，可以直接设定文件的值
    directory_node：递归的设定目录权限，默认为系统默认权限
    force：如果目标主机包含该文件，但内容不同，如果设置为yes，则强制覆盖；如果设置为no，则只有当目标主机的目标位置不存在该文件时，才复制。默认为yes
    others：所有file模块里的选项都可以在这里使用
    src：要复制到远程主机的文件在本地的地址，可以是绝对路径，也可以是相对路径。

    用法： ansible fangwei -m copy -a 'src=/root/ninngenn.cfg dest=/root/ninngenn.cfg force=yes';

   4. File模块
    File模块主要是对于文件的一些简单操作，主要是创建或者权限设定，已经文件的存在判断等。

    用法： ansible fangwei（定义的组） -m file -a 'path=/root/ninngenn.cfg state=touch';

   5. Script模块
    这个模块用于在受管机上执行sh脚本。

    用法：ansible test_hosts（定义的组） -m script -a '/tmp/script.sh';
    ```