# QEMU的构建

```./configure --target-list=riscv64-softmmu && make```

构建后的二进制文件在```qemu/build/```

```qemu-system-riscv64```用于运行完整的RISCV64系统，```qemu-riscv64```用于直接执行RISCV64的可执行程序。