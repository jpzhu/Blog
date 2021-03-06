# 将raw映像转成vmware映像
在做系统开发的时候，我们通常会拿到一个raw格式的映像文件。你可以将它烧到usb或sd卡中去运行，随时更新非常方便。  
对于x86平台，有时我们也会有这样的需求，就是将该映像转成vmware格式的，可以让其在VMPlayer里运行。qemu-img命令，提供了这种帮助.  
假如我们拿到了一个映像名叫logan-A9-CrossvilleLapis-rootfs-mmcblk1p.raw，可以通过如下命令生成一个名为a9.vmdk的vmware映像.  

>qemu-img convert -f raw logan-A9-CrossvilleLapis-rootfs-mmcblk1p.raw -O vmdk a9.vmdk

如果需要对文件映像里的内容，比如启动脚本，kernel等进行改动。VMWare提供了一个vmware-mount的命令可以很方便的做这个工作。

**查看分区表**  
要对一个映像里的内容进行mount，则需知道该分区的情况,vmware-mount -p可以查看分区信息。  
[jpzhu@WR ]$ vmware-mount -p A9_VMware.vmdk

    Nr Start Size Type Id Sytem
    – ———- ———- —- — ————————
    1 1 262143 BIOS 83 Linux
    2 262144 4194304 BIOS 83 Linux
    3 4456448 4194304 BIOS 83 Linux
    4 8650752 5345284 BIOS F Win95 Ext’d (LBA)
    5 8650753 2097152 BIOS 83 Linux
    6 10747905 2097153 BIOS 5 Extended
    7 10747906 2097152 BIOS 83 Linux
    8 12845058 1048577 BIOS 5 Extended
    9 12845059 1048576 BIOS 83 Linux
    10 13893635 102401 BIOS 5 Extended
    11 13893636 102400 BIOS 83 Linux

**mount指定分区**  
命令格式是  
vmware-mount 映像名字 分区号 mount目录

例如，第一个分区是通常是boot分区，我们把它映射到一个boot_vm目录里  
[jpzhu@WR A9_VMware]$ vmware-mount A9_VMware.vmdk 2 boot_vm/

**umount指定分区**  
umount命令是 vmware-mount -d,后面跟mount上的目录就可以  
[jpzhu@WR A9_VMware]$ vmware-mount -d boot_vm/

**umount所有已经mount的分区**  
如果进行了很多次mount操作，而且你已经切换了工作目录, vmware-mount -x可以帮助方便地umount所有通过vmware-mount命令mount上的目录。  
[jpzhu@WR A9_VMware]$ vmware-mount -x
