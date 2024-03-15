# 基于家居机器人的应用端业务系统研究

## 1.绪论



### 研究背景



### 国外研究现状



### 国内研究现状



### 本章小结



## 2.ARM开发板移植嵌入式系统

 目前我们在以AR9341（A53 核）微处理器为核心的硬件平台上，构建嵌入式linux软件平台，在此基础上在进行二次开发。在搭建环境部分主要做的工作：下载配置编译交叉编译链，建立交叉编译环境。分析bootloader的启动过程原理，这个基础上对一个版本的 uboot 进行移植研究分析实现从nand flash启动；总结一个版本的内核的新特性，分析内核移植的要点，移植、配置、编译内核；研究驱动模块，实现网络驱动；移植、配置、编译文件系统完成嵌入式环境的搭建；

在机器人上开发程序功能，需要先有一个操作系统环境，然后才能搞；

### 编译交叉编译工具

使用的交叉编译工具的版本有讲究，因为需要跟商用的软件用的库函数的编译环境匹配；选择合适的cross-version.tar.gz，解压安装到一个目录，假设是```/usr/ocal/arm/```目录下。

需要配置主机服务：minicom（串口使用工具）、tftp（简单文件传输）、nfs（mount挂在文件夹使用的）

### 编译BootLoader

bootloader是系统上电后运行的第一段代码。在一个基于arm的嵌入式系统中，这是一个非常重要的系统组成部分。Bootlader主要适用于**初始化硬件设备**，**建立内存空间的映射图**、**将操作系统内核复制到内存中并跳转到内核入口**。

有一个开放源码，支持linux的一些bootloader的列表；

一般有两种启动模式，**Flash启动模式**：先将内核映像和根文件系统写入Flash，设备启动时Bootloader将Flash中的内核及根文件系统映像读入SDRAM指定位置并跳转到内核入口执行内核，整个过程没有用户接入。

**下载模式**：这种模式下，Flash中可以没有内核映像和根文件系统。设备启动时BootLoader通过串口或网络等通信方式从主机下载内核映像和根文件系统映像等。
（1）做为安装或更新手段。通过bootloader将要写入flash的文件下载到内存，然后通过相应的命令将其写到flash指定的地址，在第一次安装内核与根文件系统时被使用，以后系统更新时也会使用bootloader的这种工作模式。
（2）做为调试手段。将内核和根文件下载到内存指定位置，并配置好传递给内核的参数，可用bootloader提供的命令直接运行内核，此种方式，可以避免将内核和根文件频繁写入flash。

论文作者刷入bootloader的方式是：在板子上有一个nor flash和一个nand flash，可以把bootloader烧写到nand flash， 然后在nor运行。

我用到的机器人的板子上的bootloader应该是厂商给的uboot，然后我们刷入就行吧；

**介绍下Uboot的目录结构，**

**移植分析：**大多数bootloader都分为stage1和stage2两部分，uboot也不例外。依赖于cpu体系结构的代码通常放在stage1用汇编实现，stage2则采用c语言来写，可以实现更复杂的功能，而且有更好的可读性和可移植性。

**uboot从nand flash中启动：**修改板级代码，如makefile、include下的目标板.h头文件（一些寄存器地址等）、board目录下的目标板.c文件，flash.c文件，u-boot.lds链接文件（链接.o文件成一个image文件），还有cpu目录下的串口驱动文件。





### 编译系统内核

内核的一些特性，包括支持的处理器、抢占式内核，交互更快、修改了I/O子系统部分等等

简述linux内核的启动流程

移植的实现：（1）初始阶段，尽量屏蔽不相关设备驱动及内核功能配置选项，构造最小内核。（2）需要改动的文件：Makefile，arch/arm目录下的config.in、makefile、mm和mach-s3c2410等，需要设置的信息包括一些**交叉编译链**，**config来配置需要的模块**，**分区信息**等；





### 编译根文件系统



### 其他配置

## 3.模块说明

### 3.1不同编程语言的交互







### 3.2通信模块

因为分成三个部分，所以会加上一个

#### 配网流程

#### 测试留下的HTTP通信机制

创建一个线程池，然后用这个线程池中的线程做监听，监听窗口  ```httpPort```，

```

httpExecutorService = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());

httpServer = HttpServer.create(new InetSocketAddress(httpPort),10);
httpServer.setExecutor(httpExecutorService);
httpServer.createContext("/",new RootHandler());

httpServer.start();

```

不同的请求信息，对应到不同的处理类，把这个类放到map中，当相应的 请求 过来的时候用对应的处理类做处理；

```
ConcurrentHashMap<String,IrayHttpHandler> requestMap = new ConcurrentHashMap<>();
```

#### 视觉板之间的通信





#### 导航板之间的通信

### 3.3数据库交互

可以先讲一下传统的连接，缺点等

然后我们使用的 hutools

数据库有哪些内容，作用



## 4.启动流程

可以先放一个整体架构，带上交互之类的，这里可以参考之前看过的一些论文。





## 5.测试和验证

可以写几个 demo 验证通信，然后做一个验证，或者在机器人上直接 copy 效果下来。























