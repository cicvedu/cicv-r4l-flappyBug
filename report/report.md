## 作业1：编译Linux内核
步骤：在linux文件夹下，
```
make x86_64_defconfig
make menuconfig # 勾选rust support
make -j$(nproc)
```
![](./compile-kernel.png)

## 作业2：对Linux内核进行一些配置

### Q&A:
1. 编译成内核模块，是在哪个文件中以哪条语句定义的？
    Makefile中`$(MAKE) -C $(KDIR) M=$$PWD`
2、该模块位于独立的文件夹内，却能编译成Linux内核模块，这叫做out-of-tree module，请分析它是如何与内核代码产生联系的？
    在Makefile中，我们指定了内核源代码的位置（KDIR），并告诉make在那里查找内核的Makefile。然后，我们使用M=$$PWD来告诉内核Makefile我们的模块源代码在哪里。这样，内核的Makefile就可以编译我们的模块了。


