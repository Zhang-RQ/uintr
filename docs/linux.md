# Linux启动环境构建

获取Linux内核代码, OpenSBI代码, BusyBox代码。请将它们放置在同一目录：

```
.
├── busybox-1.33.1
├── linux
├── opensbi
├── qemu
├── rootfs
└── rootfs.img
```

构建riscv-gnu-toolchain以实现交叉编译。（文档见```toolchain.md```）

使用上述工具链编译Linux内核：
```
make ARCH=riscv CROSS_COMPILE=riscv64-unknown-linux-gnu- defconfig
make ARCH=riscv CROSS_COMPILE=riscv64-unknown-linux-gnu- -j $(nproc)
```

配置BusyBox：```make CROSS_COMPILE=riscv64-unknown-linux-gnu- menuconfig```, 要选中```Settings-Build Options-Build static binary (no shared libs)```，并进行构建：

```
make CROSS_COMPILE=riscv64-unknown-linux-gnu- -j $(nproc)
make CROSS_COMPILE=riscv64-unknown-linux-gnu- install
```

编译后_install文件夹下生成了busybox的可执行文件。

接下来构建Linux文件系统，首先创建QEMU镜像并格式化为ext4：

```
qemu-img create rootfs.img 1g
mkfs.ext4 rootfs.img
mkdir rootfs
sudo mount -o loop rootfs.img rootfs
```

将编译的busybox可执行文件复制到构建的文件系统中，并创建必要的文件夹：

```
cd rootfs
sudo cp -r ../busybox-1.33.1/_install/* .
sudo mkdir tmp proc sys dev etc etc/init.d
```

将工具链编译出的工具和库文件复制到文件系统中：

```
sudo cp -r /opt/riscv/sysroot .
```

在 `etc/init.d` 文件夹下创建 `rcS` 并修改文件的执行权限 `sudo chmod u+x rcS`：

```
#!/bin/sh

mount -t proc none /proc
mount -t sysfs none /sys
echo -e "\nBoot took $(cut -d' ' -f1 /proc/uptime) seconds\n"
/sbin/mdev -s
```

最后卸载rootfs.img: ```sudo umount rootfs```

使用QEMU运行Linux：

```
#!/bin/bash

QEMU=./qemu/build/riscv64-softmmu/qemu-system-riscv64
LINUX=./linux/arch/riscv/boot/Image
ROOTFS=./rootfs.img
BIOS=./opensbi/build/platform/generic/firmware/fw_dynamic.bin

$QEMU \
    -M virt -m 256M -nographic \
    -smp 4 \
    -kernel $LINUX \
    -drive file=$ROOTFS,format=raw,id=hd0 \
    -device virtio-blk-device,drive=hd0 \
    -append "root=/dev/vda rw console=ttyS0" \
    -bios $BIOS \
    -D qemu-linux.log \
```
