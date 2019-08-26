---
layout: post
title: Linux Kernel Module Basic
categories:
  - Linux Kernel
---
The codes on the post are from the Linux kernel module programming guide [^Salzman].

A Linux kernel module is a piece of compiled binary code that is inserted directly into the Linux kernel [^Robert]. The important kernel module commands are as follows:

- `insmod`: Install a module
- `rmmod`: Delete a module
- `lsmod`: Get information of loaded modules by reading `/proc/modules`
- `modprobe`: 'Intelligently' add and remove modules from the kernel




[^Salzman]: Peter Jay Salzman, Micheal Burian and Ori Pomerantz, "The Linux Kernel Module Programming Guide," 2001.
[^Robert]: https://blog.sourcerer.io/writing-a-simple-linux-kernel-module-d9dc3762c234
