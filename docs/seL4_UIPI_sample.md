目前的uipi_sample实现在seL4test框架中，seL4test的框架分为两部分：driver和test, 其中driver负责运行环境构建，而test就是需要运行的一些测试程序。

该框架会将所有测试按顺序执行，且不支持多线程。

目前我的实现基本和原来的uipi_sample实现一致，使用pthread创建一个sender线程用来发送中断。做了一些关于系统调用的修改，但是这些修改不影响整个程序的运行逻辑。

seL4test的readme在这个链接: [seL4test](https://github.com/seL4/sel4test)。
