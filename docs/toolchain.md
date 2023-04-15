# RISC-V GNU工具链搭建

首先下载RISC-V GNU工具链的源码：https://github.com/riscv-collab/riscv-gnu-toolchain

在clone上述repo后还需要checkout所有submodule。（需要~6.65GB空间）

以Ubuntu为例，安装如下依赖包：

```bash
sudo apt-get install autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev ninja-build
```

配置编译选项：

```bash
./configure --prefix=/opt/riscv --enable-multilib
```

(这里建议开启multilib)

进行编译：

```
sudo make linux
```

(安装到/opt/riscv需要root权限)

这一步时间较长，大约需要2小时时间。

**注意：不能开启并行编译(make -j)**