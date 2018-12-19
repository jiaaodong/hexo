---
title: 配置一台Ubuntu18.04和Windows10双系统的电脑并配置好Tensorflow
date: 2018-12-17 02:12:41
tags:
---
## 1. 安装双系统
### 1.1 制作usb启动盘：
下载Ubuntu18.04的iso并用工具（比如rufus）burn到USB-key中，这时候usb-key就成了启动盘。
### 1.2 Windows分区：
- 先把Windows的快速启动关掉。
- 然后在硬盘分区中把Windows系统盘分出一部分来作为unallocated space。
- 最后按住shift键重启，选择trouble shooting->UEFI那一项->重启。
### 1.3 Ubuntu安装
- 重启会进入BIOS，微星的是click BIOS 5，是一个可以用鼠标操作的GUI，在里面找到boot的顺序（可以拖动的小图标），主板就是按照这个顺序来启动系统的，把usb-key放到hard-disk之前。
- 然后关闭（点右上角的x，会有提示出来你做了哪些修改，点yes）
- 然后就进入了usb-key的启动状态，选install ubuntu。
- 全部都点continue，直到选择哪种方式安装，选择something else，然后手动分三个区，两个类型ext4分别挂在/和/home，第三个类型为swap，建议为RAM的两倍。
- 然后continue，等着安装就行了。
- 安装完了就会问你是否重启，点重启
- 重启后就还是会进入usb-key，我暂时还没找到别的更快的办法，我的办法就是进入“试用ubuntu”，然后关机，等电脑关机后，拔掉usb-key，再开机，然后就会直接进入Windows。
- 按照之前的方法重启进入BIOS（按住shift后重启，找troubleshooting，点advanced，找UEFI重启）
- 在BIOS里找Advanced（右上角），然后找settings（左上角），找boot，找boot的优先级中设置硬盘的（下面的几条中的一条），然后把硬盘的boot优先级设置为先Ubuntu再windows boot manager。
- 设置完了推出BIOS（右上角x，出现的对话框点yes）
- 这时候就会进入GRUB了——一个决定你用哪个系统来boot的工具。
### 1.4 Pitfalls
1. 卸载ubuntu时候不能在windows中直接把分区释放，这样会导致再开机时候grub报错，解决办法是用usb启动盘选择try ubuntu，进去后作为一个living的操作系统，用boot repair把grub修复。
2. 将ubuntu设为硬盘的优先启动时候，如果用usb-key启动盘，会报No living system错误，如果出现这样的错误可以从Windows进BIOS进USB启动盘。
3. ubuntu重新分区的问题：必须用USB启动盘打开gparted，并且分区只能按照内存的顺序来安排。
4. 不要瞎设置BIOS中的其他选项，我把graphics configuration的第一项设置成了IGD，直接导致我显示屏接到显卡上就啥信号都收不到了，接到主板上重新进入BIOS才弄了回来....
## 2. 安装Anaconda
官网下载安装Anaconda，同意conda修改bashrc（如果不希望系统默认用conda启动python可以拒绝，在某些情况下不同的包管理器可能会有冲突，比如Anaconda和ROS）
Anaconda安装完毕后安装Conda建议安装的VSCode
在Anaconda中给自己的项目创建一个新的环境，什么环境都行，为了以后不用base环境，比如叫做my_envs
我一开始是用`conda create -n my_env_name`这种方法来创建，后来发现创建的文件夹中什么都没有（没有python解释器），才意识到Anaconda不仅仅是python的包管理器，它创建的environments如果不指定用哪种语言的话，就是个空的envs。所以用`conda create -n my_env_name python=3.7`这种形式。


## 3. 配置VSCode
如果找不到想要的环境，可以在settings里面找到venvPath选项，点右上角的`{}`可以直接修改json，总之就是把你想加的解释器加到这里就行。
## 4. 安装驱动、CUDA & CUDnn
### 4.1 驱动
不要考虑单独装驱动了，太烦了，如果用software & update里面自动找到的驱动，是nvidia-390，如果用runfile要费好大事。最好的办法是：直接用CUDA Toolkit 10装。但是在这之前，一定要按照[这里写的把secure boot给关掉！！！！！](https://wiki.ubuntu.com/UEFI/SecureBoot/DKMS)否则，安装nvidia-390后打开nvidia-settings没显卡，打开system settings-> details显示的是主板的显卡，即使用CUDA Toolkit10安装包装上的是410，但是点nvidia-settings的时候，打都打不开。
所以，一定要禁用secure boot。
### 4.2 CUDA
安装CUDA 10！
### 4.3 cuDNN
cudnn给18.04有三个，三个都要安装上才行。verify installation的方法因为权限的问题和官网要求的结果不同，但好像也不太影响使用。
## 5. 安装tensorflow
1. 用pip3安装是没办法装到conda虚拟环境里的！！！！！
2. 用python3.7的pip是找不到tensorflow的，要用python 3.6 ！！！！！
3. 在CUDA10和对应的cuDNN下，不能装tensorflow-gpu，必须要用tf-nightly-gpu！！！！


