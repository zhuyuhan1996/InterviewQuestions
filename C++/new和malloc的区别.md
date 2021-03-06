new/delete是C++关键字，需要编译器支持。malloc/free是库函数，需要头文件支持（保存在stdlib.h中）

1.参数

使用new操作符申请内存分配时无须指定内存块的大小，编译器会根据类型信息自行计算。而malloc则需要显式地指出所需内存的尺寸。

2.返回类型

new操作符内存分配成功时，返回的是对象类型的指针，类型严格与对象匹配，无须进行类型转换，故new是符合类型安全性的操作符。而malloc内存分配成功则是返回void * ，需要通过强制类型转换将void*指针转换成我们需要的类型

3.分配失败

new分配失败是返回bad_alloc异常，malloc分配失败时返回NULL。

4.自定义类型

new会先调用operator new函数，申请足够的内存（通常底层使用malloc实现）。然后调用类型的构造函数，初始化成员变量，最后返回自定义类型指针。delete先调用析构函数，然后调用operator delete函数释放内存（通常底层使用free实现）。

malloc/free是库函数，只能动态的申请和释放内存，无法强制要求其做自定义类型对象构造和析构工作。

5.重载

C++允许重载new/delete操作符，特别的，布局new的就不需要为对象分配内存，而是指定了一个地址作为内存起始区域，new在这段内存上为对象调用构造函数完成初始化工作，并返回此地址。而malloc不允许重载。

6.内存区域

new操作符从自由存储区上为对象动态分配内存空间，而malloc函数从堆上动态分配内存，自由存储区是C++上的一个基于new操作符的抽象概念，凡是通过new操作符进行内存申请的，该内存为自由存储区。而堆是操作系统中的术语，是操作系统所维护的一块特殊内存，用于程序的内存动态分配。

malloc：

作用是在内存的动态存储区中分配一个长度为size的连续空间。

void *malloc (long numbytes)：该函数负责分配 numbytes 大小的内存，并返回指向第一个字节的指针

void free(void *firstbyte)：如果给定一个由先前的 malloc 返回的指针，那么该函数会将分配的空间归还给进程的“空闲空间”。