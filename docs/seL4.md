# seL4在QEMU上运行

安装或编译支持RISC-V的QEMU。

编译RISC-V GNU工具链，并且在编译时要加入```--enable-multilib```选项。

获取seL4test代码：

```
repo init -u https://github.com/seL4/sel4test-manifest.git
repo sync
```

编译：

```
mkdir cbuild
cd cbuild
../init-build.sh -DPLATFORM=spike -DRISCV64=1 -DSIMULATION=1
ninja
```

运行：

```
./simulate
```

