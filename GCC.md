# GCC

GCC:GNU Compiler Collection(GUN 编译器集合)，它可以编译C、C++、JAV、Fortran、Pascal、Object-C、Ada等语言。

gcc是GCC中的GUN C Compiler（C 编译器）

g++是GCC中的GUN C++ Compiler（C++编译器）

## gcc和g++区别

1. 对于 .c和.cpp文件，gcc分别当做c和cpp文件编译（c和cpp的语法强度是不一样的）

2. 对于 .c和.cpp文件，g++则统一当做cpp文件编译

3. 使用g++编译文件时，**g++会自动链接标准库STL，而gcc不会自动链接STL**

4. gcc在编译C文件时，可使用的预定义宏是比较少的

5. gcc在编译cpp文件时/g++在编译c文件和cpp文件时（这时候gcc和g++调用的都是cpp文件的编译器），会加入一些额外的宏，这些宏如下：

```C++
#define __GXX_WEAK__ 1
#define __cplusplus 1
#define __DEPRECATED 1
#define __GNUG__ 4
#define __EXCEPTIONS 1
#define __private_extern__ extern
```

6. 在用gcc编译c++文件时，为了能够使用STL，需要加参数 –lstdc++ ，但这并不代表 gcc –lstdc++ 和 g++等价，它们的区别不仅仅是这个

主要参数

```shell
-g - turn on debugging (so GDB gives morefriendly output)

-Wall - turns on most warnings

-O or -O2 - turn on optimizations

-o - name of the output file

-c - output an object file (.o)

-I - specify an includedirectory

-L - specify a libdirectory

-l - link with librarylib.a
```

使用示例：g++ -ohelloworld -I/homes/me/randomplace/include helloworld.C

## gcc编译命令

### 常用编译命令选项

假设源文件名为`test.c`

- `gcc test.c` 将test.c预处理、汇编、编译并链接形成可执行文件。未指定默认为`a.out`

- `-o`  指定输出文件名。该参数可以省略。默认下生成名为`a.out`的可执行文件。

  ` gcc test.c -o test` 将test.c预处理、汇编、编译并链接形成可执行文件。

- `-E`  只预编译。直接输出编译结果

  `gcc -E test.c -o test.i` 只预编译，将`test.c`预处理输出`test.i`文件。

- `-S`  只执行到源代码到汇编代码的转换，输出汇编代码。

  `gcc -S test.i`  将预处理输出文件`test.i`汇编成`test.s`文件

- `-c`  只执行到编译，输出目标文件。生成.o文件(进行编译，不链接)。

  `gcc -c test.s ` 将汇编输出文件test.s编译输出test.o文件。

  `gcc -c sourceFile.c -o outputFileName`生成名为sourceFile.o的目标文件。

- `-O`  编译器对代码进行自动优化编译，输出效率更高的可执行文件。级别越大优化效果越好，编译时间越长。

  ​	-O 后面还可以跟上数字指定优化级别，如： `gcc -O2 source_file.c `一般可选择2；3会有一定风险。

- `-s` 效果与`strip`相同，把正常编译中的一些额外信息(如函数名)去除。用`strip`命令重新将debug信息清除。

### 库依赖

​	函数库实际上就是一些头文件和库文件的集合.

- `-I`选项向gcc的头文件搜索路径中添加新的目录
- `-L`选项向gcc的库文件搜索路径中添加新的目录
- `-l`指定链接的库文件名，如 -ldavid 表示指示gcc去连接库文件libdavid.so
- `-static`强制使用静态链接库
- `-share` 此选项将尽量使用动态库，所以生成文件比较小，但是需要系统由动态库. 

动态库文件(.so)和静态库文件(.a)的区别在于程序执行时所需的代码是运行时动态加载的,还是编译时静态加载的.gcc优先使用动态库

### gcc参数的警告提示功能

- `-pedantic` 使用了ANSI/ISO C语言扩展语法的地方将产生相应的警告信息
- `-Wall`    产生尽可能多的警告信息,建议始终带上
- `-Werror`   将所有的警告当成错误进行处理

### 其他

- `-g`　生成供调试用的可执行文件，可以在GDB中运行。 　

- `-C` 　在预处理的时候,不删除注释信息,一般和-E使用,有时候分析程序，用这个很方便的 　　

- `-M `　生成文件关联的信息。包含目标文件所依赖的所有源代码你可以用gcc -M hello.c 来测试一下，很简单。 

- `-pipe` 管道,它可以用来同时连接两个程序,其中一个程序的输出将直接作为另一个程序的输入,这样就可以避免使用临时文件,提高编译速度,但编译时消耗更多的内存.

- `-D` 定义指定的宏，使它能够通过源码中的#ifdef进行检验

  ​	`gcc -D MAX_SIZE=value sourceFile.c `  预定义名为MAX_SIZE ，值为value的宏。默认为1

- `-g3`    获得有关调试程序的详细信息

- `-v`    启动所有报警

- `-w`    禁止所有报警

## 多源文件编译方法

假设有两个源文件为`test.c`和`testfun.c`

### 多个文件一起编译

用法：`# gcc testfun.c test.c -o test`

作用：将`testfun.c`和`test.c`分别编译后链接成`test`可执行文件

### 分别编译各个源文件，之后对编译后输出的目标文件链接

用法：

```shell
gcc -c testfun.c 	//将testfun.c编译成testfun.o
gcc -c test.c		//将test.c编译成test.o
gcc -o testfun.o test.o -o test //将testfun.o和test.o链接成test
```

以上两种方法相比较，第一种方法编译时需要所有文件重新编译，而第二种方法可以只重新编译修改的文件，未修改的文件不用重新编译。

## 库文件链接

​		通常需要借助许多库函数的支持来完成相应的功能。从程序员角度看，库函数实际就是一些头文件`.h`和库文件(`so`或`lib、dll`)的集合。虽然大多数函数都morning将头文件放到`usr/include/(Linux)`下，库文件放到`usr/lib/(Linux)`下。但有时候用到的库也不在这些目录下，所以在gcc编译的时候需要用自己的方法来找到需要的头文件和库文件。

例：程序test.c在linux上使用c连接mysql。其中include文件夹路径是`/usr/dev/mysql/include/`，lib文件夹是`usr/dev/mysql/lib`

### 编译成可执行文件

编译`test.c`为目标文件

```shell
gcc -c -I /usr/dev/mysql/include test.c -o test.o
```

### 链接

把所有目标文件链接成可执行文件

```shell
gcc -L /usr/dev/mysql/lib -lmysqlclient test.o -o test
```

### 强制链接时使用静态连接库

默认情况下，GCC在链接时优先使用动态链接库，只有当动态链接库不存在时才考虑使用静态链接库，如果需要的话在编译时加上`-static`选项强制使用静态链接库。

```shell
gcc –L /usr/dev/mysql/lib –static –lmysqlclient test.o –o test
```

### 库链接路径

**静态库链接时搜索路径顺序**：

1. ld会去找GCC命令中的参数-L
2. 再找gcc的环境变量LIBRARY_PATH
3. 再找内定目录 /lib /usr/lib /usr/local/lib 这是当初compile gcc时写在程序内的

**动态链接时、执行时搜索路径顺序:**

1. 编译目标代码时指定的动态库搜索路径
2. 环境变量LD_LIBRARY_PATH指定的动态库搜索路径
3. 配置文件/etc/ld.so.conf中指定的动态库搜索路径
4. 默认的动态库搜索路径/lib
5. 默认的动态库搜索路径/usr/lib

**有关环境变量**：
LIBRARY_PATH环境变量：指定程序静态链接库文件搜索路径
LD_LIBRARY_PATH环境变量：指定程序动态链接库文件搜索路径

# GDB

## 