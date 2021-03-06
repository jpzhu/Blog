# Android kernel的编译，以kunlun为例
如果在Android项目里不想编译整个工程，只想编译Kernel部分的代码，则不能在kernel目录里简单地使用make或mm命令。

两个步骤

>$ cp arch/arm/configs/omap3_kunlun_n710e_defconfig .config  
>$ make -j 4 ARCH=arm CROSS_COMPILE=/home/zhu/kunlun/prebuilt/ti/kernel_cross_compile/arm-2010q1/bin/arm-none-linux-gnueabi-

那为什么从顶级目录执行make的时候，不需要自己手动设置这些变量的，原因还是在kernel目录里的Android.mk. 我们看kunlun/kernel/Android.mk

    9 KERNEL_CONFIG := $(TARGET_KERNEL_CONFIG)
    10 kernel_MAKE_CMD := make -j 4 -C $(LOCAL_PATH) ARCH=$(TARGET_ARCH) CROSS_COMPILE=$(ANDROID_BUILD_TOP)/$(TARGET_KERNEL_CROSSCOMPILE)
    ......
    22 $(LOCAL_PATH)/.config: $(LOCAL_PATH)/arch/$(TARGET_ARCH)/configs/$(KERNEL_CONFIG)
    23 $(hide) echo "Configuring kernel with $(KERNEL_CONFIG).."
    24 $(hide) $(kernel_MAKE_CMD) $(KERNEL_CONFIG)

从22行可以看到.config是从$(KERNEL_CONFIG)拷贝过来的，而$(KERNEL_CONFIG)来自于变量$(TARGET_KERNEL_CONFIG)【第9行】  
从10行就是我们要执行的完整命令，其中我们也有我们关心的$(TARGET_ARCH)和$(TARGET_KERNEL_CROSSCOMPILE)

而这些$TARGET开头的变量都是在平台相关的脚本里设置的。在kunlun项目里，这些地址的路径在  
kunlun/device/wrs/kunlun_n710e/

在编译的时候执行lunch kunlun_n710e的时候，会将系统的这些变量设置上，从而后面的编译能自动用上。  
同理，如果你准备移植一个新平台，device下的这个目录是挺重要的:)
