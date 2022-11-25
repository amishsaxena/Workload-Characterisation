** Rough Journal to note some observations **

> Use the Documentation file for better and clear information. This is just for testing and trying out random things. 

1. Alter command to boot up the bare kernel, according to the [Link](https://nickdesaulniers.github.io/blog/2018/10/24/booting-a-custom-linux-kernel-in-qemu-and-debugging-it-with-gdb/) but similar to Yuankun's page: 
```
$ qemu-system-x86_64 -enable-kvm -kernel arch/x86/boot/bzImage -smp cores=1 -m 2048 -append "console=ttyS0" -initrd ramdisk.img -nographic 
```
  - But somehow this messes up my terminals text lining. So we are probably better with the other -display none ??
  - Some changes have been observed though. However we can try using the terminal in full screen mode to minize the effect of the messed terminal window.
  - `qemu-system-x86_64 -enable-kvm -kernel arch/x86/boot/bzImage -smp cores=1 -m 2048 -append "console=ttyS0" -initrd ramdisk.img -nographic -cpu host` : This does the same job too, no idea what `-cpu host` does...?



