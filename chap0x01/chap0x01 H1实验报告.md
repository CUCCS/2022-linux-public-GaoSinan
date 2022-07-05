# H1实验报告

### 实验环境

VMware Workstation

- Ubuntu 20.04 Server 64bit

阿里云 云起实验室 提供的【零门槛云上实践平台】

### 实验问题

1. 调查并记录实验环境的如下信息：

   当前 Linux 发行版基本信息

   当前 Linux 内核版本信息

2. Virtualbox 安装完 Ubuntu 之后新添加的网卡如何实现系统开机自动启用和自动获取 IP？

3. 如何使用 `scp` 在「虚拟机和宿主机之间」、「本机和远程 Linux 系统之间」传输文件？

4. 如何配置 SSH 免密登录？

### 实验过程

1. ##### 查看当前Linux发行版基本信息

   命令如下：

   ```
   lsb_release -a
   cat /etc/issue
   ```

   在本地虚拟机中：

   ![](.\img\Ubuntu_local.png)

   在阿里云实践平台中：

   ![](.\img\Ubuntu_cloud.png)

2. ##### 查看当前Linux内核版本信息

   命令如下：

   ```
   uname -a
   ```

   在本地虚拟机中：

   ![](.\img\uname -a.png)

   

3. ##### 新添加的网卡实现系统开机自动启用和自动获取 IP

   使用命令`ifconfig -a`查看所有网卡

   使用命令`ifconfig`查看目前正在工作的网卡

   ![](.\img\inet.png)

   使用vim打开文件`sudo vim /etc/netplan/00-installer-config.yaml`

   手动给没有工作的网卡写入参数参数`dhcp4: true`

   最后执行`sudo netplan apply`

   参考链接：https://blog.csdn.net/xiongyangg/article/details/110206220

   理论上是这样，但在实际操作中，我发现显示找不到vim命令，几番尝试之后发现是没有安装，按照指令安装之后打开。

   ![](.\img\error_vim.png)

   与示例不同，我的文件中是一片空白。与此同时，我意识到，我的网卡都在工作，似乎无需配置。

   ![](C:\Users\萘思\Desktop\H1\img\blank.png)

4. ##### 使用 `scp` 在「虚拟机和宿主机之间」、「本机和远程 Linux 系统之间」传输文件

   ```
   scp local_filename root@remote_ip:remote_folder
   
   scp root@remote_ip:remote_file_path local_path
   ```

   出现了Connection refused的问题，经查询后确定应为ssh未配置安装的问题，尝试安装，又出现了无法定位软件包的问题，尝试解决。

   ![](.\img\无法定位软件包.png)

   一层套一层的问题出现，一个一个去解决，结果过回到最初，还是没有成功。

   参考链接：

   scp命令：https://blog.csdn.net/qq_41873311/article/details/125010228

   解决connection refused问题：https://blog.csdn.net/KlD1412/article/details/85288270

   出现“E: 无法定位软件包问题”解决方法：https://blog.csdn.net/qq_43716281/article/details/120104953

5. ##### 配置 SSH 免密登录

   按参考的去配置，上来yum就有了问题，又是一层套一层的问题

   ![](.\img\error_yum.png)

   ![](.\img\final.png)

   ![](.\img\ssh_set.png)

   参考链接：

   ssh连接报错：Connection refused详细解决办法：https://blog.csdn.net/l_mloveforever/article/details/118734694
   
   安装yum：https://blog.csdn.net/m0_67394230/article/details/123771780
   
   安装yum报错【无法定位软件包yum】：https://blog.csdn.net/pingyan158/article/details/121992571
   
   SSH免密登录配置：http://t.zoukankan.com/hanwen1014-p-9048717.html
