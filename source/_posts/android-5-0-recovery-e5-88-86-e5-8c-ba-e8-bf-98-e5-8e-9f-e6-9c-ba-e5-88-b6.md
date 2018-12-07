---
title: Android 5.0 Recovery分区还原机制
tags:
  - Android
url: 26.html
id: 26
categories:
  - Android
  - C/C++
date: 2016-06-02 11:05:34
---

0x00 背景
-------

近段时间在做OTA升级项目中发现，单独对recovery分区刷入新的版本后，启动进入到系统后，再进入recovery模式，会发现recovery被还原为之前的版本，导致校验结果失败。

0x01 真相
-------

首先，Andorid编译过程中会生成system/recovery-from-boot.p，该包是由recovery.img与boot.img计算产生的patch包，用于还原recovery分区准备的。 其次，Android编译输出目录下，root目录的init.rc加入如下启动项： service flash\_recovery /system/bin/install-recovery.sh class main seclabel u:r:install\_recovery:s0 oneshot 我们再来看看这个install-recovery.sh脚本，如下： #!/system/bin/sh if ! applypatch -c EMMC:/dev/block/bootdevice/by-name/recovery:32059648:5ab7bc2f80c9c6f98f444b2ff8e72277ceb7e142; then applypatch -b /system/etc/recovery-resource.dat EMMC:/dev/block/bootdevice/by-name/boot:31394048:34dd9856663d4a11a8eaaafd422e10ce88c03b47 EMMC:/dev/block/bootdevice/by-name/recovery 5ab7bc2f80c9c6f98f444b2ff8e72277ceb7e142 32059648 34dd9856663d4a11a8eaaafd422e10ce88c03b47:/system/recovery-from-boot.p && log -t recovery “Installing new recovery image: succeeded” || log -t recovery “Installing new recovery image: failed” else log -t recovery “Recovery image already installed” fi

0x02 总结
-------

通过上述脚本，发现系统每次启动时会判断recovery分区的摘要值是否正确，如果正确则在logcat中打印“Recovery image already installed”，如果不正确，则会执行applypatch命令，首先判断boot分区的摘要值是否正确，如果正确则使用recovery-from-boot.p补丁包进行recovery分区的恢复，恢复成功打印logcat日志”Installing new recovery image: succeeded”，恢复失败则打印”Installing new recovery image: failed”。 通过以上分析，根据具体系统定制的需求，可在编译时把该启动脚本屏蔽掉即可。