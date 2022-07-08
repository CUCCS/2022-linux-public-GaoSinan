# H3实验报告

### 实验环境

本地虚拟机环境Ubuntu 20.04

### 实验问题

- [Systemd 入门教程：命令篇 by 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)

- [Systemd 入门教程：实战篇 by 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)

- 自查清单

  如何添加一个用户并使其具备sudo执行程序的权限？

  如何将一个用户添加到一个用户组？

  如何查看当前系统的分区表和文件系统详细信息？

  如何实现开机自动挂载Virtualbox的共享目录分区？

  基于LVM（逻辑分卷管理）的分区如何实现动态扩容和缩减容量？

  如何通过systemd设置实现在网络连通时运行一个指定脚本，在网络断开时运行另一个脚本？

  如何通过systemd设置实现一个脚本在任何情况下被杀死之后会立即重新启动？实现***杀不死\***？

### 实验过程

##### [Systemd 入门教程：命令篇 by 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)

启动服务

`$ sudo /etc/init.d/apache2 start`

`或者`

`$ service apache2 start`

![](.\img\apache2 not found.png)

查看 Systemd 的版本

 `systemctl --version`

![](.\img\version.png)

###### systemctl

```bash
# 重启系统
$ sudo systemctl reboot

# 关闭系统，切断电源
$ sudo systemctl poweroff

# CPU停止工作
$ sudo systemctl halt

# 暂停系统
$ sudo systemctl suspend

# 让系统进入冬眠状态
$ sudo systemctl hibernate

# 让系统进入交互式休眠状态
$ sudo systemctl hybrid-sleep

# 启动进入救援状态（单用户状态）
$ sudo systemctl rescue
```

###### systemd-analyze

```bash
# 查看启动耗时
$ systemd-analyze                                                                                       

# 查看每个服务的启动耗时
$ systemd-analyze blame

# 显示瀑布状的启动过程流
$ systemd-analyze critical-chain

# 显示指定服务的启动流
$ systemd-analyze critical-chain atd.service
```

[![asciicast](https://asciinema.org/a/AgllNmzocXmY03rkthFwMaV7w.svg)](https://asciinema.org/a/AgllNmzocXmY03rkthFwMaV7w)

###### hostnamectl

```bash
# 显示当前主机的信息
$ hostnamectl

# 设置主机名。
$ sudo hostnamectl set-hostname rhel7
```

[![asciicast](https://asciinema.org/a/JuqoBYWJAZqSSGqrpnIMmnvj3.svg)](https://asciinema.org/a/JuqoBYWJAZqSSGqrpnIMmnvj3)

######  localectl

```bash
# 查看本地化设置
$ localectl

# 设置本地化参数。
$ sudo localectl set-locale LANG=en_GB.utf8
$ sudo localectl set-keymap en_GB
```

[![asciicast](https://asciinema.org/a/v1A8HVt0pgB6jpkYFA7WT9aFG.svg)](https://asciinema.org/a/v1A8HVt0pgB6jpkYFA7WT9aFG)

###### timedatectl

```bash
# 查看当前时区设置
$ timedatectl

# 显示所有可用的时区
$ timedatectl list-timezones                                                                                   

# 设置当前时区
$ sudo timedatectl set-timezone America/New_York
$ sudo timedatectl set-time YYYY-MM-DD
$ sudo timedatectl set-time HH:MM:SS
```

[![asciicast](https://asciinema.org/a/fsKAaho2gOW1RpuAT9C54uuBE.svg)](https://asciinema.org/a/fsKAaho2gOW1RpuAT9C54uuBE)

###### loginctl

```bash
# 列出当前session
$ loginctl list-sessions

# 列出当前登录用户
$ loginctl list-users

# 列出显示指定用户的信息
$ loginctl show-user ruanyf
```

[![asciicast](https://asciinema.org/a/EfaabX2sjBU2qekh5opdzul8S.svg)](https://asciinema.org/a/EfaabX2sjBU2qekh5opdzul8S)

###### systemctl list-units

```bash
# 列出正在运行的 Unit
$ systemctl list-units

# 列出所有Unit，包括没有找到配置文件的或者启动失败的
$ systemctl list-units --all

# 列出所有没有运行的 Unit
$ systemctl list-units --all --state=inactive

# 列出所有加载失败的 Unit
$ systemctl list-units --failed

# 列出所有正在运行的、类型为 service 的 Unit
$ systemctl list-units --type=service
```

[![asciicast](https://asciinema.org/a/oXQcobl7ax54XgX2ofA6L041P.svg)](https://asciinema.org/a/oXQcobl7ax54XgX2ofA6L041P)

###### systemctl status

```bash
# 显示系统状态
$ systemctl status

# 显示单个 Unit 的状态
$ systemctl status bluetooth.service

# 显示远程主机的某个 Unit 的状态
$ systemctl -H root@rhel7.example.com status httpd.service
```

```bash
# 显示某个 Unit 是否正在运行
$ systemctl is-active application.service

# 显示某个 Unit 是否处于启动失败状态
$ systemctl is-failed application.service

# 显示某个 Unit 服务是否建立了启动链接
$ systemctl is-enabled application.service
```

[![asciicast](https://asciinema.org/a/AqoS0OgMKCppfYfm6NA6PM0hy.svg)](https://asciinema.org/a/AqoS0OgMKCppfYfm6NA6PM0hy)

###### Unit管理

```bash
# 立即启动一个服务
$ sudo systemctl start apache.service

# 立即停止一个服务
$ sudo systemctl stop apache.service

# 重启一个服务
$ sudo systemctl restart apache.service

# 杀死一个服务的所有子进程
$ sudo systemctl kill apache.service

# 重新加载一个服务的配置文件
$ sudo systemctl reload apache.service

# 重载所有修改过的配置文件
$ sudo systemctl daemon-reload

# 显示某个 Unit 的所有底层参数
$ systemctl show httpd.service

# 显示某个 Unit 的指定属性的值
$ systemctl show -p CPUShares httpd.service

# 设置某个 Unit 的指定属性
$ sudo systemctl set-property httpd.service CPUShares=500
```



###### 依赖关系

`systemctl list-dependencies`命令列出一个 Unit 的所有依赖。

> ```bash
> $ systemctl list-dependencies nginx.service
> ```

上面命令的输出结果之中，有些依赖是 Target 类型（详见下文），默认不会展开显示。如果要展开 Target，就需要使用`--all`参数。

> ```bash
> $ systemctl list-dependencies --all nginx.service
> ```



