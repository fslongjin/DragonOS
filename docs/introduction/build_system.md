# 构建DragonOS

## 软件依赖

- GNU make
- GCC >= 8.3.0
- xorriso
- grub 2.06

## 开发环境

​    目前，DragonOS在Deepin V20上进行开发。经测试，在Debian bullseye上，可以正常编译、运行。建议使用Docker运行debian镜像进行开发。（后期将会发布开发环境的docker镜像）

## 运行环境

1. qemu 6.2.0（编译安装并启用gdb调试选项）
2. gdb
3. VNC Viewer

## 编译DragonOS

1. 安装编译及运行环境
2. 进入DragonOS文件夹
3. 输入命令：`make -j 16`即可编译

## 运行DragonOS

### 安装软件依赖

&emsp;&emsp;在运行DragonOS之前，需要先安装需要先安装上述软件依赖。

### 创建磁盘镜像

#### 概述

&emsp;&emsp;使用tools目录下的脚本，创建一至少为16MB磁盘镜像（类型选择raw）。并建立MBR分区表，然后将第一个分区格式化为FAT32分区。

&emsp;&emsp;在完成以上操作后，将创建的磁盘文件移动至bin文件夹（若不存在，则需要您手动创建），并将其重命名为“disk.img”

&emsp;&emsp;最后，在DragonOS目录下运行 `bash run.sh`脚本，将会完成编译、文件拷贝、内核镜像打包、启动qemu虚拟机的全过程。当qemu虚拟机启动后，即可使用VNC Viewer连接到虚拟机。

#### 具体操作方法

&emsp;&emsp;首先，您需要使用`sudo`权限运行`tools/create_hdd_image.sh`，为DragonOS创建一块磁盘镜像文件。该脚本会自动完成创建磁盘镜像的工作，并将其移动到`bin/`目录下。

**原理**

&emsp;&emsp;该脚本在创建磁盘镜像之后，会自动调用fdisk，您需要在fdisk之中对虚拟磁盘进行初始化。您需要使用fdisk把磁盘的分区表设置为MBR格式，并创建1个分区。具体操作为：输入o、n, 然后按4次回车，直到回到fdisk的默认界面，再输入w即可，按顺序输入，并且，每次输入完成后要按下回车。完成操作后，磁盘镜像`disk.img`将会被创建。
    
&emsp;&emsp;至此，准备工作已经完成，您可以运行`run.sh`，然后DragonOS将会被启动。