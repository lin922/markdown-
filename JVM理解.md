C++编译是一步到位的， 直接生成了机器可执行文件一次编写，到处编译

java编译是生成的中间文件，可以放到各个操作系统所定制的java虚拟机中去，之后再进-步编译成机器语言并执行，所以Java可跨平台，它的跨平台是建立在有对应JVM基础之上的。一次编写，到处执行

- JDK = JAVA的运行环境（JVM+Java系统类库）和JAVA工具

- JRE = JAVA虚拟机JVM和JAVA程序所需的核心类库

- JVM = (java Virtual Machine Java虚拟机)

1、JRE（Java Runtime Environment  java运行环境）

包括JAVA虚拟机和JAVA程序所需的核心类库，如果想要运行一个开发好的JAVA程序，计算机中只要安装JRE即可

2、JDK（Java Development  toolKit  java开发工具包）

JDK是提供给JAVA开发人员使用的，其中包含了JAVA的开发工具，也包括了JRE。所以安装了JDK，就不用再单独安装JRE了

3、JVM(java Virtual Machine Java虚拟机)

Java号称是一次编写，到处运行。也就是说，Java程序可以实现跨平台，在Windows上写好的Java程序，可以运行在Linux或者其它平台上面，而不用修改源代码。而C或者C++就不行了，他是跟平台相关的。Java只所以能够跨平台，是因为Java程序不是直接运行在操作系统上的，而是运行在JVM上的。而JVM根据不同的操作系统，有不同的版本，比如有Linux版本的，Windows版本的等。我们在安装JRE或者JDK的时候，需要根据操作系统来下载不同的版本，而JDK和JRE里面已经包括了JVM，上面也说过了。所以，Java程序才能够实现跨平台！

JAVA语言的三种结构

1、J2EE(Java 2 Platform Enterprise Edition)企业版  javaee

是为开发企业环境下的应用程序提供的一套解决方案。 该技术体系中包含的技术如 Servlet Jsp等，主要针对于Web应用程序开发。可以使用一些现有的框架来快速的做企业网站的开始，比如SSH框架

2、J2SE（Java 2 Platform Standard Edition）标准版javase

是为开发普通桌面和商务应用程序提供的解决方案。 该技术体系是其他两者的基础，可以完成一些桌面应用程序的开发。 比如Java版的扫雷。它是学习J2EE或J2ME的基础，主要包括了Java的基本语法规范，面向对象等内容。

3、J2ME(Java 2 Platform Micro Edition)小型版

是为开发电子消费产品和嵌入式设备提供的解决方案。 该技术体系主要应用于小型电子消费类产品，如手机中的应用程序等。

Java5.0版本后，更名为 JAVAEE   JAVASE   JAVAME