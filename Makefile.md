#是注释

###### #第一层：显示规则

#目标文件：依赖文件

#[TAB]指令

伪目标：.PHONY：

###### #第二层：变量

=（替换） +=（追加） ：=（恒等于）

TAR = test

OBJ = circle.o cube.o main.o

CC := gcc

使用时可以将设置好的变量替换为：$(TAR)  $(OBJ)  $(CC)

###### #第三层：隐含规则

%.c   %.o 任意的.c或.o      *.c   *.o 所有的.c   .o

###### 第四层：通配符 

$^所有的依赖文件   $@所有的目标文件

示例：

![image-20220315155636375](C:\Typora\mylearning\\img\image-20220315155636375.png)

![image-20220315155704585](C:\Typora\mylearning\\img\image-20220315155704585.png)

![image-20220315161250400](C:\Typora\mylearning\\img\image-20220315161250400.png)