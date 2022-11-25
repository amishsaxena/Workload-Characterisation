Install bison and flex and libelf-dev 
(if using *debian based* system, these are not present by default and are required for kernel compilation)

- make help : Gives some idea of what to do
- *Everythin needs to be done in Linux Branch inside* `Workload-Characterisation/linux/linux-5.17.4` .

### Configuration 

2 Options : `make tinyconfig` or `menuconfig`

0. (Needs to be done only for one time) : Create an init ramfs
	`mkinitramfs -o ramdisk.img` [Link](https://yuankun.me/posts/running-raw-linux-kernel-in-qemu/)

1. tinyconfig for now. try menuconfig if doesnt work.
	* Doesnt work*
2. use `make defconfig`

3. To make the kernel use `make -j8` or `make -j4`

4. The images is presen-t at : 
	`Kernel: arch/x86/boot/bzImage is ready  (#2)`
	
5. Run it using the system instlled qemu (it is added to the ENV, so we use it as it is for now, later try on our custom built qemu). Command is : 

```
$ qemu-system-x86_64 -enable-kvm -kernel arch/x86/boot/bzImage -smp cores=1 -m 2048 -append "console=ttyS0" -initrd ramdisk.img -serial stdio -display none

```
6. This makes the kernel boot into the basic initramfs mode. From here we need to start building it. 

### Reference links : 

- https://www.josehu.com/memo/2021/01/02/linux-kernel-build-debug.html
- https://nickdesaulniers.github.io/blog/2018/10/24/booting-a-custom-linux-kernel-in-qemu-and-debugging-it-with-gdb/
- https://medium.com/@daeseok.youn/prepare-the-environment-for-developing-linux-kernel-with-qemu-c55e37ba8ade
- https://github.com/cirosantilli/linux-kernel-module-cheat
- https://yuankun.me/posts/running-raw-linux-kernel-in-qemu/
