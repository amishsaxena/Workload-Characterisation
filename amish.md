** Rough Journal to note some observations **

> Use the Documentation file for better and clear information. This is just for testing and trying out random things. 

1. Alter command to boot up the bare kernel, according to the [Link](https://nickdesaulniers.github.io/blog/2018/10/24/booting-a-custom-linux-kernel-in-qemu-and-debugging-it-with-gdb/) but similar to Yuankun's page: 
```
$ qemu-system-x86_64 -enable-kvm -kernel arch/x86/boot/bzImage -smp cores=1 -m 2048 -append "console=ttyS0" -initrd ramdisk.img -nographic 
```
  - But somehow this messes up my terminals text lining. So we are probably better with the other -display none ??
  - Some changes have been observed though. However we can try using the terminal in full screen mode to minize the effect of the messed terminal window.
  - `qemu-system-x86_64 -enable-kvm -kernel arch/x86/boot/bzImage -smp cores=1 -m 2048 -append "console=ttyS0" -initrd ramdisk.img -nographic -cpu host` : This does the same job too, no idea what `-cpu host` does...?


### Nov 26 

- QEMU ccode in linux branch does not build 
- So use the QEMU code the builds in the Master branch.
- Now run `./qemu-system-x86_64 -kernel ../../../../Workload-Characterisation/linux/linux-5.17.4/arch/x86/boot/bzImage -smp cores=1 -m 2048 -append "console=ttyS0" -initrd ../../../../Workload-Characterisation/linux/linux-5.17.4/ramdisk.img  -nographic` to check if kernel just runs on the custom QEMU build. 
  - Seems to be working.
- Now try running with the tracelog plugin and it just keeps printing the instructions to console. No idea if its actually running. Printing is slowing down things.


### Dec 4

Tried various methods to attach -hda but not sure how to create the image to attach into the -hda and append. On doing the following : 

`qemu-system-x86_64 -no-kvm -kernel arch/x86/boot/bzImage -initrd ramdisk.img -smp 1 -m 2048 -append "console=ttyS0 root=/dev/sda" -hda myos.img  -nographic`

The kernel still boots to initramfs and shows that /dev/sda is mounted. 

The myos.img was made as : 
```
qemu-img create myos.img 512M
mkfs.ext4 myos.img 
 
```

> Something WORKED !!

- Using *Alpine Linux* images for now (light, minimal and arch based)
- make `alpine.qcow2` image from Alpine Linux standard iso file as shown [here](https://wiki.alpinelinux.org/wiki/Install_Alpine_in_QEMU)
- Make sure to do proper setup.
- Boot in qemu using : 
`qemu-system-x86_64 -enable-kvm -kernel arch/x86/boot/bzImage -append "root=/dev/sda3" -m 512 -hda alpine.qcow2`


