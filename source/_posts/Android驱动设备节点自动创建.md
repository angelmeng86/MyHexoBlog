---
title: Android驱动设备节点自动创建
tags:
  - android
categories:
  - 移动端开发
date: 2016-10-19 11:08:15
---

x00 背景
-------

最近帮同事解决一个问题，就是在Ubuntu（主机）下编译的USB驱动程序，在设备热插拔的时候会自动创建设备节点，但是该驱动程序移植到Android系统下，则需要手动创建节点，该节点还需要手工配置DAC和MAC安全策略，对于实际使用中非常麻烦；

0x01 问题分析及解决
------------

参见文章《[Android Uevent 分析，从kernel到framework](http://blog.chinaunix.net/uid-24545924-id-3128349.html) 》 我们知道Android上层的uevent事件使用了自个的一套实现，接下来我们开发分析代码，源码文件：system/core/init/devices.c
```
static void handle\_generic\_device_event(struct uevent *uevent)
{
    char *base;
    const char *name;
    char devpath\[DEVPATH_LEN\] = {0};
    char **links = NULL;

    INFO("lwz usb: generic\_device\_event start\\n");
    
    name = parse\_device\_name(uevent, 64);
    if (!name)
        return;
    //打印收到的热插拔事件信息
    INFO("maple usb1: device\_name:%s, major:%d, minor:%d, action:%s name:%s\\n",  uevent->device\_name, uevent->major, uevent->minor, uevent->action, name);
    struct ueventd_subsystem *subsystem =
            ueventd\_subsystem\_find\_by\_name(uevent->subsystem);

    if (subsystem) {
        const char *devname;
        switch (subsystem->devname_src) {
        case DEVNAME\_UEVENT\_DEVNAME:
            devname = uevent->device_name;
            break;

        case DEVNAME\_UEVENT\_DEVPATH:
            devname = name;
            break;

        default:
            ERROR("%s subsystem's devpath option is not set; ignoring event\\n",
                    uevent->subsystem);
            return;
        }

        if (!assemble_devpath(devpath, subsystem->dirname, devname))
            return;
        mkdir\_recursive\_for_devpath(devpath);
    } else if (!strncmp(uevent->subsystem, "usb", 3)) {
         if (!strcmp(uevent->subsystem, "usb")) {
            if (uevent->device_name) {
                if (!assemble\_devpath(devpath, "/dev", uevent->device\_name))
                    return;
                mkdir\_recursive\_for_devpath(devpath);
             }
             else {
                 /\* This imitates the file system that would be created
                  \* if we were using devfs instead.
                  \* Minors are broken up into groups of 128, starting at "001"
                  */
                 int bus_id = uevent->minor / 128 + 1;
                 int device_id = uevent->minor % 128 + 1;
                 /\* build directories */
                 make_dir("/dev/bus", 0755);
                 make_dir("/dev/bus/usb", 0755);
                 snprintf(devpath, sizeof(devpath), "/dev/bus/usb/%03d", bus_id);
                 make_dir(devpath, 0755);
                 snprintf(devpath, sizeof(devpath), "/dev/bus/usb/%03d/%03d", bus\_id, device\_id);
             }
         } else {
             /\* ignore other USB events */
             //忽略一些不符合上述条件的USB事件，并返回
        INFO("maple usb: ignore other USB events\\n");
             //我这里是通过判断创建的节点名称是否是我们想要的，如果是则继续往下执行进行节点的创建和移除操作
         if(!strncmp(uevent->device_name, "myusb", 5)) {
        base = "/dev/";
             }
             else
             return;
         }
     } else if (!strncmp(uevent->subsystem, "graphics", 8)) {
         base = "/dev/graphics/";
         make_dir(base, 0755);
     } else if (!strncmp(uevent->subsystem, "drm", 3)) {
         base = "/dev/dri/";
         make_dir(base, 0755);
     } else if (!strncmp(uevent->subsystem, "oncrpc", 6)) {
         base = "/dev/oncrpc/";
         make_dir(base, 0755);
     } else if (!strncmp(uevent->subsystem, "adsp", 4)) {
         base = "/dev/adsp/";
         make_dir(base, 0755);
     } else if (!strncmp(uevent->subsystem, "msm_camera", 10)) {
         base = "/dev/msm_camera/";
         make_dir(base, 0755);
     } else if(!strncmp(uevent->subsystem, "input", 5)) {
         base = "/dev/input/";
         make_dir(base, 0755);
     } else if(!strncmp(uevent->subsystem, "mtd", 3)) {
         base = "/dev/mtd/";
         make_dir(base, 0755);
     } else if(!strncmp(uevent->subsystem, "sound", 5)) {
         base = "/dev/snd/";
         make_dir(base, 0755);
     } else if(!strncmp(uevent->subsystem, "misc", 4) &&
                 !strncmp(name, "log_", 4)) {
         kernel_logger();
         base = "/dev/log/";
         make_dir(base, 0755);
         name += 4;
     } else if (!strncmp(uevent->subsystem, "dvb", 3)) {
         /\* This imitates the file system that would be created
          \* if we were using devfs instead to preserve backward compatibility
          \* for users of dvb devices
          */
         int adapter_id;
         char dev_name\[20\] = {0};

         sscanf(name, "dvb%d.%s", &adapter\_id, dev\_name);

         /\* build dvb directory */
         base = "/dev/dvb";
         mkdir(base, 0755);

         /\* build adapter directory */
         snprintf(devpath, sizeof(devpath), "/dev/dvb/adapter%d", adapter_id);
         mkdir(devpath, 0755);

         /\* build actual device directory */
         snprintf(devpath, sizeof(devpath), "/dev/dvb/adapter%d/%s",
                  adapter\_id, dev\_name);
     } else
         base = "/dev/";
     links = get\_character\_device_symlinks(uevent);
     if (!links)
         links = get\_v4l\_device_symlinks(uevent);

     if (!devpath\[0\])
         snprintf(devpath, sizeof(devpath), "%s%s", base, name);
    //开始进行热插拔处理，包括设备节点的创建和移除，可参考相关代码继续分析
    INFO("maple usb: devpath %s, major:%d, minor:%d, action:%s\\n", devpath, uevent->major, uevent->minor, uevent->action);
     handle_device(uevent->action, devpath, uevent->path, 0,
             uevent->major, uevent->minor, links);
}
```