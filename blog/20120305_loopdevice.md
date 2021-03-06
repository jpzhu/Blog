# 创建一个loopdevice 文件
在我们工作中经常会拿到别人已经生成好的一个image文件，可以直接通过dd命令烧录到usb或sdk等设备中。  
这样的设备文件，其实是一个loop device文件，所以通常我们会用mount -o loop这样的命令来读取这样的文件。  
我们也可以根据需求自己来制作这样的一个image定制。  
我们以做一个16M的文件为例子。  

**利用dd命令产生一个16M的文件**  
dd if=/dev/zero bs=4M count=4 of=my.img

**将该文件关联到loop device设备上**  
linux 可以关联的loop设备总共有8个，从loop0到loop8  
sudo losetup /dev/loop0 my.img

**格式化该image**  
如果你的这个image不需要分区信息的话，只需执行以下命令就可以进行格式化了  
sudo mkfs.ext3 /dev/loop0

**为image建立分区**  
如果，你的这个image要做的比较复杂，还需要进行分区，比如/boot,/,/home等，那么需要用fdisk命令对该loop设备进行分区  
sudo fdisk /dev/loop0

**格式化image中对应的分区**  
要格式化对应loop0中不同的分区，那么需要显视的可以看见对应的分区，kpartx命令可以帮助我们  
sudo kpartx -av /dev/loop0

因为，我在fdisk命令的时候创建了两个分区，所以执行完kpartx命令的时候可以在/dev/mapper下看到有两个分区loop0p1和loop0p2  
[jpzhu@WR tmp]$ ls /dev/mapper/loop0p?

>/dev/mapper/loop0p1 /dev/mapper/loop0p2

此时，你可以对loop0p1和loop0p2进行分别的格式化。

**清理现场**  
因为刚才在执行操作的时候，将image文件关联到loop device上，当完全工作后，我们需要解除这种关联  
[jpzhu@WR tmp]$ sudo kpartx -d /dev/loop0  
[jpzhu@WR tmp]$ sudo losetup -d /dev/loop0