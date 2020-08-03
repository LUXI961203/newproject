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



