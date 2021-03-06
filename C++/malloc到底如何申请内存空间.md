malloc()到底从哪里得到了内存空间？

答案是从堆里面获得空间。也就是说函数返回的指针是指向堆里面的一块内存。

​      操作系统中有一个记录空闲内存地址的链表。当操作系统收到程序的申请时，就会遍历该链表，然后就寻找第一个空间大于所申请空间的堆结点，然后就将该结点从空闲结点链表中删除，并将该结点的空间分配给程序。

​     malloc函数的实质体现在，它有一个将可用的内存块连接为一个长长的列表的所谓空闲链表（Free List）。调用malloc函数时，它沿连接表寻找一个大到足以满足用户请求所需要的内存块(根据不同的算法而定（将最先找到的不小于申请的大小内存块分配给请求者，将最合适申请大小的空闲内存分配给请求者，或者是分配最大的空闲块内存块）。然后，将该内存块一分为二（一块的大小与用户请求的大小相等，另一块的大小就是剩下的字节）。接下来，将分配给用户的那块内存传给用户，并将剩下的那块（如果有的话）返回到连接表上。

​     调用free函数时，它将用户释放的内存块连接到空闲链上。到最后，空闲链会被切成很多的小内存片段，如果这时用户申请一个大的内存片段，那么空闲链上可能没有可以满足用户要求的片段了。于是，malloc函数请求延时，并开始在空闲链上翻箱倒柜地检查各内存片段，对它们进行整理，将相邻的小空闲块合并成较大的内存块。如果无法获得符合要求的内存块，malloc函数会返回NULL指针，因此在调用malloc动态申请内存块时，一定要进行返回值的判断。

在此也要说明就是因为new和malloc需要符合大众的申请内存空间的要求，针对泛型提供的，分配内存设计到分配算法和查找，此外还要避免内存碎片，所以其效率比较低下，因此有时程序猿会自己重写new和delete，或者创建一个内存池来管理内存，提高程序运行的效率。