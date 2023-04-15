# Uintr-IPC-bench使用文档

首先获取项目源码：

```
git clone https://github.com/Zhang-RQ/uintr-ipc-bench
```

使用CMake进行配置，这里会使用riscv-gnu-toolchain：

```
mkdir build && cd build
cmake -DCMAKE_TOOLCHAIN_FILE=RISCV.cmake ..
```

进行编译：

```
make
```

注意：请将编译出的```build/source```文件夹复制到rootfs.img的```/tests/ipc-bench```中。