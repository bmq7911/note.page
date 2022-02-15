# Makefile

1. makefile是一种脚本语言,它拥有一般语言的某些特性,也有自己的特性

2. 目标:makefile的主要目标是管理编译过程,和任何程序类似,本质上都是对数据进行处理并产生输出;

   makefile通过rule来描述输入到输出的整个过程,其格式如下:

   ```makefile
   target ... : prerequesities ...
   	recipe
   	...
   	...
   ```

   本质上也就是:

   ```
   输出 : 输入
   	根据输入产生输出的方法
   ```

   其中recipe前必须是一个tab字符

   ```
   main:main.c
   	gcc main.c -o main
   ```

   这就描述了从源文件main.c 到 main的过程,当我们使用 make main时,makefile会根据目标(main)去查血makefile的目标(targets),并根据rule生成我们的目标(使用gcc编译mian.c生成main)

2. 变量:

   makefile拥有变量,makefile之中的变量是弱类型的;

   - 3.1 变量的引用:通过\$加()来引用变量;例如: $(variable)
   
   - 3.2 变量的赋值:makefile之中变量的赋值有多种赋值方式
   
     - 3.2.1 ''**=**'' 赋值: 
   
       ```makefile
       CC=arm-linux-guneabi-gcc
       all:
       	echo $(CC) #输出 arm-linux-guneabi-gcc
       ```
   
     - 3.2.2 "**+=**" 追加赋值:
   
       ```makefile
       objs=main.o
       objs+=test_string.o
       all:
       	echo $(CC) #输出 main.o test_string.o,注意两个值之间存在空格
       ```
   
     - 3.2.3 "**?=**" 条件赋值:如果变量为空,则赋值
   
       ```makefile
       CC=arm-linux-guneabi-gcc
       CC?=gcc
       all:
       	echo $(CC) #输出 arm-linux-gnueabi-gcc
       ```
   
     - 3.2.4 "**:=**" 覆盖赋值: 如下代码
   
       ```makefile
       CC=gcc
       CC:=arm-linux-guneabi-gcc
       all:
       	echo $(CC)       #输出 arm-linux-guneabi-gcc,将覆盖原来的赋值
       ```
   
   - 3.3 特殊变量
   
     - 3.3.1 "$@" 目标变量:
   
       ```makefile
       main.o:main.c
       	gcc main.c -c -o main.o
       test.o:test.c
       	gcc test.c -c -o test.o
       main:main.o test.o
       	gcc main.o test.o -o main
       ```
   
       在执行 make main时,我们会先构建 main.o 和 test.o,在构建main.o 和 test.o的过程中,他们使用recipe是基本相同的,除了输入的源文件和输出文件不同而已,这时我们可以合并两条规则,将这两条针对具体的输入和输出的rule改变为一条通用rule
   
       ```makefile
       %.o:%.c
       	gcc $< -c -o $@
       main.o:main.c
       test.o:test.c
       main:main.o test.o
       	gcc main.o test.o -o main
       ```
   
       在上述的makefile中
   
       ```makefile
       %.o:%.c
       	gcc $< -c -o $@
       ```
   
       就是一条通用规则,描述了.c文件如何编译成.o文件,在recipe之中
   
     - "$@" 表示目标
   
     - "$<" 表示第一个依赖
     - "$^" 表示所有依赖
     - "$?" 代表比目标还要新的依赖







