---
author: wuqt
date: 2018-09-04 11:06:50+00:00
draft: false
title: 使用 qemu 调试 linux kernel
type: post
url: /
categories:
- Linux
tags:
- kernel debug
- qemu
---


	  




在 Ubuntu 中build kernel 参考如下：



	  * 
		[Ref] [https://wiki.ubuntu.com/KernelTeam/GitKernelBuild](https://wiki.ubuntu.com/KernelTeam/GitKernelBuild) 
	

可以直接在宿主机调试kernel, 但是当发生崩溃之后，工作环境又要重新配置。



	 所以考虑用 qemu，因为它有个option: -kernel， 可以直接引导kernel。比 VirtualBox 等虚拟机更快速更方便。






	  






* * *





	  







	  * 
		尝试使用 qemu 直接启动主机上的kernel 
	
	
		    * 

    
    $ sudo qemu-system-x86_64 -kernel /boot/vmlinuz-`uname -r`


		
	

这会提示缺少文件系统  




	  * 
		可以使用 debootstrap 构建一个rootfs 

    
    IMG=qemu-image.img
    DIR=mount-point.dir
    qemu-img create $IMG 1g
    mkfs.ext2 $IMG
    mkdir $DIR
    sudo mount -o loop $IMG $DIR
    sudo debootstrap --arch amd64 jessie $DIR
    sudo umount $DIR
    rmdir $DIR


	

这里最好用一下 chroot 和 passwd 更新一下文件系统里的密码，以免启动之后登不进去。



	  







	  * 
		build kernel 

    
    git clone --depth=1 git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
    cd linux
    make x86_64_defconfig
    make kvmconfig
    make -j 8


	
	  * 
		menuconfig 使能调试信息选项

    
            [*] KGDB: kernel debugger  --->
                    <*>   KGDB: use kgdb over the serial console
            -> Compile-time checks and compiler options
                    [*] Compile the kernel with debug info


	
	  * 
		booting kernel， 使用 -s 选项 (short stand for -gdb tcp::1234)

    
    $ qemu-system-x86_64 -kernel arch/x86/boot/bzImage
                         -hda qemu-image.img
                         -append "root=/dev/sda" -s


	


	  * 
		然后在另一个终端用gdb 连接它就可以了

    
    $ gdb vmlinux
    
    (gdb) target remote localhost:1234
    (gdb) breakpoint spin_lock
    (gdb) continue


	
	  * 
		如果是在非 GUI 环境调试可以指定一下 console 和增加 --nographic 选项

    
    $ qemu-system-x86_64 -kernel bzImage
                         -append "root=/dev/sda console=ttyS0"
                         -hda qemu-image.img
                         --enable-kvm
                         --nographic


	
	  * 
		Kmemleak 用来查找内存泄漏

    
            -> Memory Debugging
                    [*]Enable kernel memory leak detector.
                    (2000) Maximum kmemleak early log entries


	



<blockquote>
	在dmesg 中可以看到，或者 

>     
>     $ echo scan >> /sys/kernel/debug/kmemleak
>     $ cat /sys/kernel/debug/kmemleak
> 
> 
</blockquote>





	  







	  * 
		使能网卡

    
    $ qemu-system-x86_64 -kernel bzImage
                         -append "root=/dev/sda console=ttyS0 single"
                         -drive file=toto.img,index=0,media=disk,format=raw
                         --enable-kvm --nographic
                         -net nic -net user,hostfwd=tcp::5555-:22


	



<blockquote>

>     
>     $ ssh -p 5555 fredo@localhost
> 
> 
</blockquote>





	  







	  * 
		More kernel debugging options
	
	  * 
		dynamic_debug:  


    
            -> printk and dmesg options
                    [*] Enable dynamic printk() support


	




	  






<blockquote>
	
> 
> 
		ftrace
	
> 
> 

>     
>             -> Tracers (FTRACE [=y])
> 
> 
	
> 
> 
		  

	
> 
> 
</blockquote>





	  






* * *





	  * 
		 [原文参考] [https://www.collabora.com/news-and-blog/blog/2017/03/13/kernel-debugging-with-qemu-overview-tools-available/](https://www.collabora.com/news-and-blog/blog/2017/03/13/kernel-debugging-with-qemu-overview-tools-available/) 
	
  


