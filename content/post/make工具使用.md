---
title: 'make工具使用'
date: Mon, 28 Mar 2016 11:58:59 +0000
draft: false
categories:
  - "Embedded"
tags: ['Linux', 'make', 'Makefile']
---

Makefile 正如其名作为一个制作文件的工具，大大方便了进行大批量文件编译时的工作流程。这里简单的记录一个make 工具的基本使用规则。

#### 1\. Makefile 基本规则：

```
 target... : prerequisites ...
          command
          ...
          ...
         -------------------------------------------------------------------------------
```

target: 为目标文件，即所要生成的对象;  
prerequisites : 为要生成target所需要的文件或是目标;  
command: 即make需要执行的命令，(任意的shell命令，即对应编译器或者平台的命令)。  
第一行表示依赖关系，第二行是规则。( 第二行必须由Tab键开头 )。

注意：  
a. 只敲入make命令默认执行第一个target。  
b. command 要用Tab键缩进

#### 2\. 比如有源文件：main.c api.c api.h

用gcc 作编译的过程：

```
gcc -c main.c     
gcc -c api.c    
gcc -o main main.o api.o
```

写成Makefile的话就是:

```
    main: main.o api.o
        gcc -o main main.o api.o
    main.o: main.c
        gcc -c main.c
    api.o: api.c api.h
        gcc -c api.c    
```

另外Makefile简化格式：  
由三个非常有用的变量  
$@--目标文件，$^--所有的依赖文件，$<--第一个依赖文件  
替换简化写成:

```
    main: main.o api.o
        gcc -o $@ $^
    main.o: main.c
        gcc -c $<
    api.o: api.c api.h
        gcc -c $<    
```

再利用一个 Makefile的缺省规则  
.c.o：  
     gcc -c $<  
表示所有的.o文件都是依赖相应的.c文件。即可再简化为：

```
    main: main.o api.o
        gcc -o $@ $^
    .c.o:
        gcc -c $<   
```

#### 再贴一个简单的Demo ：

```
BIN := test.exe
all: $(BIN)
CC = gcc
SRCS = $(wildcard \*.c)
OBJS = $(SRCS:.c=.o)

$(OBJS): $(SRCS)
	$(CC) -c $^
	@echo $(OBJS)

$(BIN): $(OBJS)
	$(CC) -o $@ $^
	@echo $@ "build success"
	
.PHONY: install all clean
	
clean:
	rm -f $(BIN) $(OBJS)
```

其它请参考：[https://swcarpentry.github.io/make-novice/reference](https://swcarpentry.github.io/make-novice/reference)