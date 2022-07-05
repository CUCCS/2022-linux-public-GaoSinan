



# H2实验报告

### 实验环境

本地虚拟机环境Ubuntu 20.04

### 实验问题

使用表格方式记录至少 2 个不同 Linux 发行版本上以下信息的获取方法，使用 asciinema 录屏方式「分段」记录相关信息的获取过程和结果

【软件包管理】在目标发行版上安装 `tmux` 和 `tshark` ；查看这 2 个软件被安装到哪些路径；卸载 `tshark` ；验证 `tshark` 卸载结果

【文件管理】复制以下`shell` 代码到终端运行，在目标 Linux 发行版系统中构造测试数据集，然后回答以下问题：

- 找到 `/tmp` 目录及其所有子目录下，文件名包含 `666` 的所有文件
- 找到 `/tmp` 目录及其所有子目录下，文件内容包含 `666` 的所有文件

```
cd /tmp && for i in $(seq 0 1024);do dir="test-$RANDOM";mkdir "$dir";echo "$RANDOM" > "$dir/$dir-$RANDOM";done
```

【文件压缩与解压缩】练习课件中 文件压缩与解压缩 一节所有提到的压缩与解压缩命令的使用方法

【跟练】 子进程管理实验

【硬件信息获取】目标系统的 CPU、内存大小、硬盘数量与硬盘容量

### 实验过程&结果

1. #### 安装asciinema

   ```
   sudo apt-get update
   sudo apt-get install asciinema
   asciinema auth
   ```

   ![](.\img\asciinema_set up.png)

2. #### 【软件包管理】

   ###### tmux安装与路径查询

   ```
   sudo apt-get install tmux
   which tmux
   ```

   [![asciicast](https://asciinema.org/a/IF8TZGtDX9m88qgr5HJtiei7l.svg)](https://asciinema.org/a/IF8TZGtDX9m88qgr5HJtiei7l)

   ###### tshark的安装、查询路径、卸载、验证

   ```
   sudo apt-get install tshark
   which tshark
   sudo apt-get remove --purge tshark
   which tshark
   ```

   [![asciicast](https://asciinema.org/a/fS5jCc0QfjdhEzuz6KZ3Jqjp7.svg)](https://asciinema.org/a/fS5jCc0QfjdhEzuz6KZ3Jqjp7)

3. #### 【文件管理】

   将`shell`代码复制到终端运行：

   ```
   cd /tmp && for i in $(seq 0 1024);do dir="test-$RANDOM";mkdir "$dir";echo "$RANDOM" > "$dir/$dir-$RANDOM";done
   ```

   ```
   cd /tmp
   sudo find ./ -name '*666*'
   sudo grep -r '666' ./
   ```

   [![asciicast](https://asciinema.org/a/eOw82TCzKRF2CXp6EDFW6exDP.svg)](https://asciinema.org/a/eOw82TCzKRF2CXp6EDFW6exDP)

4. #### 【文件压缩与解压缩】

   ###### gzip

   ```
   touch test.txt
   gzip test.txt
   gzip -d test.txt.gz
   ```

   [![asciicast](https://asciinema.org/a/EOnZHZRv1r9Pk2ZzaFMhLHaud.svg)](https://asciinema.org/a/EOnZHZRv1r9Pk2ZzaFMhLHaud)

   ###### bizp2

   ```
   bzip2 -z test.txt
   bzip2 -d test.txt.bz2
   ```

   [![asciicast](https://asciinema.org/a/g9InrWjNfcx3fcwD2WXrhRAGG.svg)](https://asciinema.org/a/g9InrWjNfcx3fcwD2WXrhRAGG)

   ###### zip

   ```
   zip test.txt.zip /tmp
   unzip test.txt.zip
   ```

   [![asciicast](https://asciinema.org/a/syhecoWFhShSHN9ed1VdYzeek.svg)](https://asciinema.org/a/syhecoWFhShSHN9ed1VdYzeek)

   ###### tar

   ```
   tar -czvf test.tar.gz test.txt
   tar -xzvf test.tar.gz
   ```

   [![asciicast](https://asciinema.org/a/M5VFlfhLFHesHJfsLMw0xM9Q9.svg)](https://asciinema.org/a/M5VFlfhLFHesHJfsLMw0xM9Q9)

   ###### 7z

   ```
   7za a test.7z /test.txt
   7za x test.7z
   ```

   [![asciicast](https://asciinema.org/a/SekPpaNDp1FCtyOD73cFs4bv7.svg)](https://asciinema.org/a/SekPpaNDp1FCtyOD73cFs4bv7)

   ###### rar

   只能解压缩`unrar x test.rar`

   [![asciicast](https://asciinema.org/a/XWkKmLtFeahQmTQ7nvDd2ry8C.svg)](https://asciinema.org/a/XWkKmLtFeahQmTQ7nvDd2ry8C)

5. #### 【跟练】 子进程管理实验

   [![asciicast](https://asciinema.org/a/cxVHawRW87rYBNBJ2ag8lRKR8.svg)](https://asciinema.org/a/cxVHawRW87rYBNBJ2ag8lRKR8)

6. 【硬件信息获取】

   获取目标系统的CPU

   ```
   cat /proc/cpuinfo
   ```

   获取内存大小

   ```
   free -h
   ```

   获取硬盘数量与硬盘容量

   ```
   df -h
   ```

   [![asciicast](https://asciinema.org/a/PuioXthPZ3CHphhmXBmpSNAfa.svg)](https://asciinema.org/a/PuioXthPZ3CHphhmXBmpSNAfa)

   



