## 作业1：编译Linux内核
### 步骤
在linux文件夹下，
```
export LLVM=1
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

### 步骤
在linux文件夹下
```bash
export LLVM=1
make menuconfig #取消勾选Intel devices, Intel(R) PRO/1000 Gigabit Ethernet support
make -j$(nproc)
cd ../src_e1000
./build_image.sh
```
![](./uncheck-intel-driver.png)

进入qemu shell后

```sh
insmod r4l_e1000_demo.ko
ip link set eth0 up
ip addr add broadcast 10.0.2.255 dev eth0
ip addr add 10.0.2.15/255.255.255.0 dev eth0 
ip route add default via 10.0.2.1
ping 10.0.2.2
```

![](./r4l_e1000_demo-output.png)

## 作业3：使用rust编写一个简单的内核模块并运行

### 步骤
1. 将rust_helloworld.rs源码粘贴到linux/samples/rust下。在pr_info!输出内容的结尾加上换行符，否则可能无法及时输出。
2. 修改linux/samples/rust/Kconfig，添加内容：
```
config SAMPLE_RUST_HELLOWORLD
	tristate "Print Helloworld in Rust"
	help
	  This option builds the hello world example for Rust.

	  If unsure, say N.
```
3. 修改linux/samples/rust/Makefile，添加内容：
```
obj-$(CONFIG_SAMPLE_RUST_HELLOWORLD)	+= rust_helloworld.o
```

4. make menuconfig，添加编译rust samples中的hello world程序。
5. make -j$(nproc)编译内核
6. 将linux/samples/rust/rust_helloworld.ko复制到 src_e1000/rootfs
7. 进入到src_e1000, `./build_image.sh`

![](./hello-world.png)