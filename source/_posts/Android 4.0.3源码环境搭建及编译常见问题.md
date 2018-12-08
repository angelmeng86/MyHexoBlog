---
title: Android 4.0.3源码环境搭建及编译常见问题
tags:
  - android
categories:
  - 移动端开发
date: 2016-06-15 19:52:50
---

一、完全编译说明

系统：Ubuntu12.04.3-i386

JDK：jdk-6u29-linux-i586

源码：iTop4412_ICS

虚拟机硬件要求：建议2GB内存，交换空间至少4GB（Android4.0以上要求），编译的硬盘空间至少25GB以上。

首先按照开发版中的说明文档进行编译环境的搭建，在上述运行环境下出现的错误及解决办法说明：

sudo apt-get install xinetd build-essential nfs-kernel-server apache2 samba git-core gnupg flex bison gperf libsdl-dev libesd0-dev libwxgtk2.6-dev build-essential zip curl libncurses5-dev zlib1g-dev cscope uboot-mkimage

sudo apt-get install lib32z1-dev

sudo apt-get install lib32ncurses5-dev

sudo apt-get install libswitch-perl

sudo apt-get install git gnupg flex bison gperf build-essential zip curl libc6-dev libncurses5-dev:i386 x11proto-core-dev libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-dri:i386 libgl1-mesa-dev g++-multilib mingw32 tofrodos python-markdown libxml2-utils xsltproc zlib1g-dev:i386 dpkg-dev lib32z1-dev lib32ncurses5-dev libswitch-perl

1、内核源码编译成功！

2、Android源码编译问题依次如下：

1）、make: *** \[out/host/linux-x86/obj/EXECUTABLES/obbtool_intermediates/Main.o\] Error 1

此处编译错误是由于ubuntu 11.10采用了GCC4.6.1导致的。

解决方法：

修改源码目录下/build/core/combo/HOST_linux-x86.mk文件：

将以下语句

HOST\_GLOBAL\_CFLAGS += -D\_FORTIFY\_SOURCE=0

修改为

HOST\_GLOBAL\_CFLAGS += -U\_FORTIFY\_SOURCE -D\_FORTIFY\_SOURCE=0

2)、frameworks/compile/slang/slang\_rs\_export_foreach.cpp:249:23: 错误： variable ‘ParamName’ set but not used \[-Werror=unused-but-set-variable\]

cc1plus: all warnings being treated as errors

make: *** \[out/host/linux-x86/obj/EXECUTABLES/llvm-rs-cc\_intermediates/slang\_rs\_export\_foreach.o\] 错误 1

解决办法：

首先，在工程根目录下，打开下面的makefile文件： $ vi frameworks/compile/slang/Android.mk 其次，在打开的makefile文件中按照下面更改(22行)： local\_cflags\_for_slang := -Wno-sign-promo -Wall -Wno-unused-parameter -Werror

修改为 local\_cflags\_for_slang := -Wno-sign-promo -Wall -Wno-unused-parameter

3）、\[out/host/linux-x86/obj/STATIC\_LIBRARIES/libMesa\_intermediates/src/glsl/linker.o\]

解决办法：

$ vi external/mesa3d/src/glsl/linker.cpp 添加： #include <cstddef>

4）、\[out/host/linux-x86/obj/STATIC\_LIBRARIES/liboprofile\_pp\_intermediates/arrange\_profiles.o\]

解决办法：

$ vim external/oprofile/libpp/format_output.h

找到 94行，把 mutable 字符串注释掉；

5）、\[out/host/linux-x86/obj/STATIC\_LIBRARIES/libgtest\_host_intermediates/gtest-all.o\]

解决办法：

$ vi external/gtest/include/gtest/internal/gtest-param-util.h 添加： #include <cstddef>

6）、\[out/host/linux-x86/obj/EXECUTABLES/test-librsloader_intermediates/test-librsloader\]

解决办法：

$vim external/llvm/llvm-host-build.mk

在文件中插入一行： LOCAL_LDLIBS := -lpthread -ldl

上述问题解决后，Android源码的编译过程应该能正确通过。

64Bit补充：

1）、\[out/host/linux-x86/obj/EXECUTABLES/aapt_intermediates/aapt\] Error 1

解决：

sudo apt-get install lib32z1-dev

2）、\[out/host/linux-x86/obj/EXECUTABLES/adb_intermediates/adb\] Error 1

解决：

sudo apt-get install lib32ncurses5-dev

二、源码模块单独编译说明

在进行下述操作之前，我们需要手工对iTop4412源码进行单独的目标平台配置，拷贝iTop4412/iTop4412_ICS/device/moto/stingray/vendorsetup.sh文件至iTop4412/iTop4412_ICS/device/samsung/smdk4x12/目录下，然后进行修改：

把

add\_lunch\_combo full_stingray-userdebug

修改为

add\_lunch\_combo full_smdk4x12-eng

最后在源码目录下执行source ./build/envsetup.sh命令进行目标编译项加载，再执行指令lunch选择full_smdk4x12-eng作为编译目标平台，至此配置进行完毕。

注：在执行source ./build/envsetup.sh命令时有可能提示build/envsetup.sh: 1: Syntax error: "(" unexpected 错误，解决方法：执行$sudo dpkg-reconfigure dash命令，并选择“否”。

Google为我们准备了另外的命令来支持编译单独的模块，以及重新打包system.img的命令。

一. 首先在Android源代码目录下的build目录下，有个脚本文件envsetup.sh，执行这个脚本文件后，就可以获得一些有用的工具：

**USER-NAME@MACHINE-NAME:~/iTop4412_ICS$source ./build/envsetup.sh**

注意，这是一个source命令，执行之后，就会有一些额外的命令可以使用：

**\- croot: Changes directory to the top of the tree.**

**\- m: Makes from the top of the tree.**

**\- mm: Builds all of the modules in the current directory.**

**\- mmm: Builds all of the modules in the supplied directories.**

**\- cgrep: Greps on all local C/C++ files.**

**\- jgrep: Greps on all local Java files.**

**\- resgrep: Greps on all local res/*.xml files.**

**\- godir: Go to the directory containing a file.**

这些命令的具体用法，可以在命令的后面加-help来查看，这里我们只关注mmm命令，也就是可以用它来编译指定目录的所有模块，通常这个目录只包含一个模块。

二. 使用mmm命令来编译指定的模块，例如Email应用程序：

**USER-NAME@MACHINE-NAME:~/iTop4412_ICS$ mmm packages/apps/Email/**

编译完成之后，就可以在out/target/product/smdk4x12/system/app目录下看到Email.apk文件了。Android 系统自带的App都放在这具目录下。另外，Android系统的一些可执行文件，例如C编译的可执行文件，放在out/target/product/smdk4x12/system/bin目录下，动态链接库文件放在out/target/product/smdk4x12/system/lib目录下，out/target/product/smdk4x12/system/lib/hw目录存放的是硬件抽象层（HAL）接口文件。

三. 编译好模块后，还要重新打包一下system.img文件，这样我们把system.img运行在模拟器上时，就可以看到我们的程序了。

**USER-NAME@MACHINE-NAME:~/iTop4412_ICS$ make snod**

四. 参照[Ubuntu](http://blog.csdn.net/luoshengyang/article/details/6559955)[上下载、编译和安装](http://blog.csdn.net/luoshengyang/article/details/6559955)[Android](http://blog.csdn.net/luoshengyang/article/details/6559955)[最新源代码](http://blog.csdn.net/luoshengyang/article/details/6559955)一文介绍的方法运行模拟器：

**USER-NAME@MACHINE-NAME:~/iTop4412_ICS$ emulator**

三、单独编译模拟器调试

make snod and emulator builds.
------------------------------

**Symptom**: When using make snod (make system no dependencies) on emulator builds, the resulting build doesn't work.

**Cause**: All emulator builds now run Dex optimization at build time by default, which requires to follow all dependencies to re-optimize the applications each time the framework changes.

**Fix**: Locally disable Dex optimizations with export WITH_DEXPREOPT=false, delete the existing optimized versions with make installclean and run a full build to re-generate non-optimized versions. After that, make snod will work.

Stale ODEX dependencies cause unbootable Android
================================================

This document is based on the code of Android 4.1.1_r6.1 (Jelly Bean).

Suppose that you want to customiz your android system, you may want to modify /system/framework/framework.jar (source files are in the directory frameworks/base). During development, let's assume that you just did a full build, and you decide to make some changes to some source files. After that, you want to build again. Ideally, the building system can correctly re-build everything that is dependent on the changes you just made. However, seems that the android building system does not do it properly. You will get an unbootable image caused by _mismatched dex signature_. If you check the log when system boots, you will see an error message like the following:

07-30 06:50:56.042: I/dalvikvm(393): DexOpt: mismatch dep signature for '/system/framework/framework.odex' 07-30 06:50:56.042: E/dalvikvm(393): /system/framework/apache-xml.jar odex has stale dependencies

The reason is that many system apks which was optimized before (ODEX) need to be rebuilt because they are dependent on system framework jar (but they are not). This causes odex mismatch error at bootup which leads to unbootable android in the worst case.

To solve this problem, you have two options:

1) Set variable WITH_DEXPREOPT to false in build/target/board/generic/BoardConfig.mk

2) Add WITH_DEXPREOPT=false when make. It's like the following:

\# make -j4 WITH_DEXPREOPT=false

上述方法已经测试通过，证明可用于模拟器的源码修改调试，需要注意的是要替换system.img、ramdisk.img、userdata.img三个文件，并重新建立avd模拟器。