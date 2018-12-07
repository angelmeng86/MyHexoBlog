---
title: Linux kernel directio（内核态实现direct IO）
tags:
  - kernel
  - linux
url: 211.html
id: 211
categories:
  - C/C++
  - Linux
date: 2016-10-19 10:42:53
---

0x00 需求
-------

近段时间在研究Strongswans，需要加入硬件算法，由于Strongswans中的IPSec通道加密依赖于内核的XFRM框架，所以需要在内核层实现加密硬件的驱动，涉及到了对设备文件的O_DIRECT操作，在测试中遇到一些问题并通过以下文章受到启发，对其进行了代码改进。 参考文章：《[内核态下实现direct IO](http://blog.csdn.net/u010059563/article/details/41655835)》

0x01 优化
-------

文章所使用的do\_mmap\_pgoff方法是内核内部方法，所以需要对内核进行修改并重新编译后才能运用于内核模块的使用。通过该方法顺藤摸瓜，我找到了一个内核导出方法，经测试能够正常使用，方法如下：

extern int vm\_munmap(unsigned long, size\_t);
extern unsigned long vm_mmap(struct file *, unsigned long,
        unsigned long, unsigned long,
        unsigned long, unsigned long);

vm\_mmap内部其实就是调用了do\_mmap_pgoff方法，原理一样，只是不需要重新编译内核，参考上述文章代码修改如下（蓝色字体）：

#include <linux/init.h>
#include <linux/kernel.h>
#include <linux/module.h>
#include <linux/mm.h>
#include <linux/fs.h>
#include <linux/time.h>
#include <asm/uaccess.h>
#include <asm/mman.h>

#define LEN 1024*64
#define COUNT 1024*1024/32
#define DIR_OUT "/home/wpp/Desktop/wpp.txt"

unsigned long run_time(struct timeval end, struct timeval start)
{
	unsigned long sec, usec;

	if(end.tv\_usec >= start.tv\_usec)
	{
		sec = end.tv\_sec - start.tv\_sec;
		usec = end.tv\_usec - start.tv\_usec;
	}
	else
	{
		sec = end.tv\_sec - start.tv\_sec - 1;
		usec = end.tv\_usec + 1000000 - start.tv\_usec;
	}
	printk("the run time is %lus%luus\\n", sec, usec);

	return sec*1000000 + usec;
}

static int \_\_init temp\_init_module(void)
{
	struct file *fp;
	char *buf;
	unsigned long i, populate, rate, time;
	struct timeval start, end;
	mm\_segment\_t old_fs;
	loff_t pos;
	unsigned long memp;

	fp = filp\_open(DIR\_OUT, O\_RDWR | O\_CREAT | O_DIRECT, 0);
	if(IS_ERR(fp))
	{
		printk("open error\\n");
		return -1;
	}

	buf = NULL;
	memp = vm\_mmap(NULL, 0, LEN, PROT\_READ | PROT\_WRITE, MAP\_PRIVATE, 0);//add by maple
	buf = (char *)memp;//add by maple
	//buf = (char *)do\_mmap\_pgoff(NULL, 0, LEN, PROT\_READ | PROT\_WRITE, MAP_SHARED, 0, &populate);
	if(NULL == buf)
	{
		printk("do\_mmap\_pgoff error\\n");
		return -1;
	}
	memset(buf, 'w', LEN-1);
	buf\[LEN-1\] = '\\n';

	old\_fs = get\_fs();
	set\_fs(KERNEL\_DS);
	pos = 0;

	do_gettimeofday(&start);
	for(i=0; i<COUNT; i++)
		vfs_write(fp, (char *)buf, LEN, &pos);
	do_gettimeofday(&end);

	time = run_time(end, start);
	rate = (2\*1024\*1000000) / time;
	printk("the rate of write no cache is %luM/s\\n", rate);

	set\_fs(old\_fs);
	filp_close(fp, NULL);
	vm_munmap(memp, LEN);//add by maple
	return 0;
}

static void \_\_exit temp\_exit_module(void)
{
	printk("Goodbye\\n");
}

module\_init(temp\_init_module);
module\_exit(temp\_exit_module);
MODULE_LICENSE("GPL");