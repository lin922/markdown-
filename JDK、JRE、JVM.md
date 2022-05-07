C++编译是一步到位的， 直接生成了机器可执行文件一次编写，到处编译

java编译是生成的中间文件，可以放到各个操作系统所定制的java虚拟机中去，之后再进-步编译成机器语言并执行，所以Java可跨平台，它的跨平台是建立在有对应JVM基础之上的。一次编写，到处执行

- ==JDK== = JAVA的运行环境（JVM+Java系统类库）和JAVA工具

- ==JRE== = JAVA虚拟机JVM和JAVA程序所需的核心类库

- ==JVM== = (java Virtual Machine Java虚拟机)

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



GC 垃圾回收：GC（Garbage Collection)：JAVA/[.NET](https://baike.baidu.com/item/.NET)中的垃圾回收器。Java是由[C++](https://baike.baidu.com/item/C%2B%2B)发展来的。它摈弃了C++中一些繁琐容易出错的东西。其中有一条就是这个GC。而[C#](https://baike.baidu.com/item/C%23)又借鉴了JAVA。

在C/C++程序中，程序员在内存中主动开辟一段相应的空间来存值。由于内存是有限的，所以当程序不再需要使用该内存空间时，就需要销毁对象并释放其所占用的内存资源，好重新利用这段空间。在C/C++中，释放无用内存空间的事情需要由程序员自己来处理。但是这样显然非常繁琐，如果有所遗漏，就可能造成资源浪费甚至内存泄露。当软件系统比较复杂，变量多的时候程序员往往就忘记[释放内存](https://baike.baidu.com/item/释放内存/10736171)或者在不该释放的时候释放内存了。

有了GC，程序员就不需要再手动的去控制内存的释放。当[Java虚拟机](https://baike.baidu.com/item/Java虚拟机/6810577)（VM）或.NET[CLR](https://baike.baidu.com/item/CLR)发觉内存资源紧张的时候，就会自动地去清理无用对象（没有被引用到的对象）所占用的内存空间（这里的说法略显粗略，事实上何时清理内存是个复杂的策略）。如果需要，可以在程序中显式地使用System.gc() / System.GC.Collect（）来强制进行一次立即的内存清理。[Java](https://baike.baidu.com/item/Java/85979)提供的GC功能可以自动监测对象是否超过了作用域，从而达到自动回收内存的目的，Java的GC会自动进行管理，调用方法：System.gc() 或者Runtime.getRuntime().gc();



 Java 和 C/C++ 的 10 条不同之处:

1. C++ 支持指针，而 Java 没有指针的概念。
2. C++ 支持多继承，而 Java 不支持多重继承，但允许一个类实现多个接口。
3. Java 是完全面向对象的语言，并且还取消了 C/C++ 中的结构和联合，使编译程序更加简洁
4. Java 自动进行无用内存回收操作，不再需要程序员进行手动删除，而 C++ 中必须由程序释放内存资源，这就增加了程序员的负担。
5. Java 不支持操作符重载，操作符重载则被认为是 C++ 的突出特征。
6. Java 允许预处理，但不支持预处理器功能，所以为了实现预处理，它提供了引入语句（import），但它与 C++ 预处理器的功能类似。
7. Java 不支持缺省参数函数，而 C++ 支持 。
8. C 和 C++ 不支持字符串变量，在 C 和 C++ 程序中使用“Null”终止符代表字符串的结束。在 Java 中字符串是用类对象（String 和 StringBuffer）来实现的
9. goto 语句是 C 和 C++ 的“遗物”，Java 不提供 goto 语句，虽然 Java 指定 goto 作为关键字，但不支持它的使用，这使程序更简洁易读。
10. Java 不支持 C++ 中的自动强制类型转换，如果需要，必须由程序显式进行强制类型转换。不会警告