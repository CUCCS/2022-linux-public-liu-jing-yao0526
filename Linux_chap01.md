# Linux_chap01

### 一、调查并记录实验环境的如下信息

- #### 当前 Linux 发行版基本信息

  命令如下：

​       `lsb_release -a`

​       `cat /etc/issue`

- #### 当前 Linux 内核版本信息

   命令如下：

   `uname -a`
   
   `cat /proc/version`

操作过程如下：（分别在本地虚拟机ub20.04.2与阿里云云上实践平台进行操作）

本地虚拟机：

![view_version](img/view_version.png)

阿里云平台：

![aliyun](img/aliyun.png)

### 二、Virtualbox 安装完 Ubuntu 之后新添加的网卡如何实现系统开机自动启用和自动获取 IP

1、通过`ip a`命令，查看虚拟机网卡

2、通过`sudo vim /etc/netplan/00-installer-config.yaml`命令进入vim编辑器，手动添加如下内容：

​      `enp0s8:`

​          `dhcp4:  true`

![vim](img/vim.png)

3、**退出vim编辑器**（先按Esc键，之后输入:x保存退出），最后执行`sudo netplan apply`生效

![sudo_netplan_apply](img/sudo_netplan_apply.png)

*****本题参考链接：https://blog.csdn.net/xiongyangg/article/details/110206220

### 三、如何使用 `scp` 在「虚拟机和宿主机之间」、「本机和远程 Linux 系统之间」传输文件

#### 本机和远程 Linux 系统之间

在本地虚拟机ub20.04.2与阿里云平台间传输文件

1、用`touch a.txt`  `touch b.txt`命令分别在阿里云平台和本地虚拟机上创建两个文本文件

​     *使用`find / -name a.txt`m命令，确认文件路径

​     *使用`whoami`命令，确认用户名

​     *使用`ip a`命令，确认ip地址

![aliyun_pre](img/aliyun_pre.png)

2、【本机从远程获取文件】用`ping`命令连接远程主机地址，成功后使用

`scp -P 22 root@101.133.160.228:/root/a.txt ./`命令，输入远程主机密码后，将远程主机的a.txt文件获取至虚拟机，之后在本地虚拟机再用`ls`命令进行确认

传输：

![ali_local_get](img/ali_local_get.png)

确认过程：

![local_confirm](img/local_confirm.png)

3、【本机向远程发送文件】用`ping`命令连接远程主机地址，成功后使用

`scp /home/cuc/b.txt root@101.133.160.228:/root`命令，输入远程主机密码后，将b.txt文件发送至远程主机，之后在远程主机再用`ls`命令进行确认

传输：

![ali_local_send](img/ali_local_send.png)

确认过程：

![ali_confirm](img/ali_confirm.png)

*****本题参考链接：[详细讲解Linux终端非常使用ssh远程连接命令和scp文件上传和下载_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ti4y177uV?spm_id_from=333.999.0.0)

#### 虚拟机和宿主机之间

1、确认主机已有ssh服务，在宿主机桌面创建se.txt文件，并打开cmd

![local_ssh](img/local_ssh.png)

2、【宿主机获取虚拟机文件】用`ping`命令连接虚拟机地址，成功后使用

`scp cuc@192.168.56.101:/home/cuc/b.txt ./`命令，输入虚拟机密码后，获取虚拟机文件至宿主机，如图可见已获取b.txt

ping:

![ping_host](img/ping_host.png)

传输：

![host_ub](img/host_ub.png)

确认：

![host_confirm](img/host_confirm.png)

3、【宿主机向虚拟机发送文件】用`ping`命令连接虚拟机地址，成功后用`cd Desktop`命令切换至文件所在的桌面，然后使用

`scp se.txt cuc@192.168.56.101:/home/cuc/`命令，输入虚拟机密码后，将se.txt成功传输至虚拟机，之后在本地虚拟机再用`ls`命令进行确认

传输：

![host_ub](img/host_ub.png)

确认过程：

![ub_confirm](img/ub_confirm.png)

*****本题参考链接：http://courses.cuc.edu.cn/course/82669/learning-activity/full-screen#/249100

### 四、如何配置 SSH 免密登录

1、输入`ssh-keygen -b 4096`生成公私钥对

![ssh-keygen](img/ssh-keygen.png)

2、输入`ssh-copy-id -i ~/.ssh/id_rsa.pub cuc@192.168.56.101`后，输入虚拟机口令，完成配置，之后可以通过

`ssh 'cuc@192.168.56.101'`进行免密登录

![ssh-copy-id](img/ssh-copy-id.png)

*****本题参考链接：[Linux 系统与网络管理(2021)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Hb4y1R7FE?p=23)

### 实验中遇到的问题及解决方案

**1、虚拟机启动失败**

如图所示，在最初启动虚拟机时，我遇到了“未能启动虚拟电脑”的问题。在畅课平台本课程的第一章课后讨论区，我在“关于虚拟机报错...”的帖子中找到了解决办法。这个错误信息可能和USB2.0控制器有关，可以选择禁用USB2.0。于是采用禁用USB2.0 步骤：Settings ---> USB设备 -----> 取消启用USB控制，即可正常启用。

![ub20.04.2_error](img/ub20.04.2_error.png)

**2、无法退出vim编辑器**

在第二题中最初使用到vim编辑器时，对老师上课所教授的“让新手退出vim”学习不够深入，直接输入:x后编辑器依然处于INSERT状态。在网上查阅相关资料进行再次学习后，我明白了应该先按Esc键再输入:x后保存退出。

参考链接：[vim编辑页面怎么退出_linux系统中如何进入退出vim编辑器_weixin_39815456的博客-CSDN博客](https://blog.csdn.net/weixin_39815456/article/details/111781459)

3、关于scp

在本次实验中，遇到较多问题的是第三题。问题主要存在于，对scp函数用法不熟悉，开始时将远程地址和本地地址混淆，且语法存在错误。参考了上述第三题链接以及老师第三次直播课所讲授的内容后，我现在本地ping远程地址，确认网络连接无误后，根据正确的语法使用scp函数，问题得以解决。
