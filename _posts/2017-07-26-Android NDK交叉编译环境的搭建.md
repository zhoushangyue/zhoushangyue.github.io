---
layout: post
title:  "Android NDK交叉编译环境的搭建"
date:   2016-04-06
excerpt: "关于NDK网上有很多介绍，不再赘述。这里只是单纯的讲一下，作为一个菜鸟，构建一个NDK交叉编译环境，并跑一个C/C++版的helloworld需要做哪些工作。"
project: true
tag:
comments: true
---

关于NDK网上有很多介绍，不再赘述。

在这里只是单纯的讲一下，作为一个菜鸟，构建一个NDK交叉编译环境，并跑一个C/C++版的helloworld需要做哪些工作。

1，下载NDK，配置环境变量

NDK下载可以去官网：http://developer.android.com/tools/sdk/ndk/index.html;

下载对应的数据包，解压并将NDK配置到环境变量：

gedit  ~/.bashrc

在.bashrc中添加你nkd-bulid的路径到PATH中，参考如下内容

#NDK必须和ndk的路径保持一致

export NDK=/home/zhoushangyue/android-ndk-r11c

export PATH=$PATH:$NDK

配置完后重新导入环境变量

source  ~/.bashrc

接下来通过命令检查是否配置成功

ndk-bulid

如果没有出现“command not found”就说明成功了。

2，制作交叉编译工具链

gedit /etc/profile

在文件末尾添加

export NDK=/home/zhoushangyue/android-ndk-r11c

export NDK_CROSS=/home/zhoushangyue/AndroidToolChain/bin

PATH=$PATH:$NDK:$NDK_CROSS

保存后重启环境

source /etc/profile

执行命令：$NDK/build/tools/make-standalone-toolchain.sh --platform=android-21 --arch=arm --install-dir=
/home/zhoushangyue/AndroidToolChain/

显示如下说明已经部署好交叉编译环境。

Auto-config: --toolchain=arm-linux-androideabi-4.9
Copying prebuilt binaries...
Copying sysroot headers and libraries...
Copying c++ runtime headers and libraries...
Copying files to: /home/zhoushangyue/AndroidToolChain
Cleaning up...
Done.

其中$NDK环境变量是NDK的安装路径，选项--platform指定Android版本的开发形式,对应版本4.4.2 API。--arch指定目标执行的架构。--install-dir指定这个新生成的文件夹即是你的交叉编译环境，和其他交叉编译工具链使用方法类似。

4，编写程序并测试

随便编写一个helloworld程序hello.c执行以下编译命令：

arm-linux-androideabi-gcc hello.c -o hello

在通过adb将可执行文件hello导入测试机执行。

执行中可能会产生

error: only position independent executables (PIE) are supported

这个时候只需要在原来的编译后加-pie -fPIE重新编译一下，生成的hello就可以使用了
