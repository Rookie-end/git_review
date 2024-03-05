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



### 编译系统内核



### 编译根文件系统



### 其他配置



## 3.其他

### 网络的连接使用

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

