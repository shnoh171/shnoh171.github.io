---
layout: post
title: Linux Kernel Module Basic
categories:
  - Linux Kernel
---
The codes on the post are from the Linux kernel module programming guide [^Salzman].

A Linux kernel module is a piece of compiled binary code that is inserted directly into the Linux kernel [^Robert]. The important kernel module commands are as follows:

- `insmod`: Install a module
- `rmmod`: Remove a module
- `lsmod`: Show status of loaded modules by reading `/proc/modules`
- `modprobe`: Add/remove modules from the kernel (consider dependency)
- `depmod`: Generate `modules.dep` and `map` files

Now let's start playing with some kernel modules!

```c
// m_hello_1.c
#include <linux/module.h> // for kernel module
#include <linux/kernel.h> // for KERN_INFO

int init_module(void) {
	printk(KERN_INFO "Hello world 1\n");
	return 0; // init_module module succeeded
}

void cleanup_module(void) {
	printk(KERN_INFO "Goodbye world 1\n");
}
```

This is old style kernel module. `init_module()` is called when the module is loaded (by executing `insmod`), and `cleanup_module()` is called when the module is removed (by executing `rmmod`). A Makefile for the source code is as follows.

```
obj-m += m_hello_1.o
all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

After writing Makefile, I can generate `m_hello_1.ko` by running `make` (run `make clean` to remove it).

Installing and removing `m_hello_1` module is done by running `insmod` and `rmmod`.

```shell
shnoh@shnoh-p6-2270kr:~/kmod$ sudo insmod m_hello_1.ko
shnoh@shnoh-p6-2270kr:~/kmod$ dmesg | grep Hello
[6000342.646421] Hello world 1

shnoh@shnoh-p6-2270kr:~/kmod$ sudo rmmod m_hello_1
shnoh@shnoh-p6-2270kr:~/kmod$ dmesg | grep Goodbye
[6000365.592283] Goodbye world 1
```

We can rename  `init_module()` and `cleanup_module()` using `__init` and `__exit` macros, respectively. The code is shown in `m_hello_2.c`.

```c
#include <linux/module.h> // for kernel module
#include <linux/kernel.h> // for KERN_INFO
#include <linux/init.h> // for macros

static int __init hello_2_init(void)
{
	printk(KERN_INFO "Hello world 2\n");
	return 0;
}

static void __exit hello_2_exit(void)
{
	printk(KERN_INFO "Goodbye world 2\n");
}

module_init(hello_2_init);
module_exit(hello_2_exit)
```
I can build, install and remove the module `m_hello_2` as same as `m_hello_1`.

[^Salzman]: Peter Jay Salzman, Micheal Burian and Ori Pomerantz, "The Linux Kernel Module Programming Guide," 2001.
[^Robert]: <https://blog.sourcerer.io/writing-a-simple-linux-kernel-module-d9dc3762c234>
