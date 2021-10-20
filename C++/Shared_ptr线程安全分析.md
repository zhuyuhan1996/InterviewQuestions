shared_ptr对象提供与内置类型相同级别的线程安全性。
1. 同一个shared_ptr对象可以被多线程同时读取
2. 不同的shared_ptr对象可以多线程同时修改（即使这些shared_ptr对象管理着同一个对象指针）。
3. 任何其他并发访问的结果都是无定义的。

- 第一种情况是对对象的并发读，自然是线程安全的。
- 第二种情况下，如果两个shared_ptr对象管理的是不同对象的指针，则这两个对象完全不相关，支持并发写也容易理解。但是如果A和B管理的是同一个对象P的指针，则A和B需要维护一块共享的内存区域，该区域记录P指针当前的引用计数。对A和B的并发写必然涉及对该引用计数内存区的并发修改。

两个线程对SP1和SP2操作：
- SP1和SP2都递增引用计数，即add_ref_copy被并发调用，也就是两个_InterlockedIncrement（&use_count_)并发执行，这是线程安全的
- SP1和SP2都递减引用计数，即release被并发调用，也就是_InterlockedDecrement(&use_count_ )并发执行，这也是线程安全的。只不过后执行的线程负责删除对象。
- SP1递增引用计数，调用add_ref_copy；SP2递减引用计数，调用release。由于SP1的存在，SP2的release操作无论如何都不会导致use_count_变为零，也就是说release中if语句的body永远不会被执行。因此，这种情况就化简为_InterlockedIncrement（&use_count_)和_InterlockedDecrement( &use_count_ )的并发执行，仍然是线程安全的。

综上所述，多线程通过不同的shared_ptr或者weak_ptr对象并发修改同一个引用计数对象sp_counted_base是线程安全的。而sp_counted_base对象是这些智能指针唯一操作的共享内存区，因此最终的结果就是线程安全的。

正如boost文档所宣称的，boost为shared_ptr提供了与内置类型同级别的线程安全性。这包括：
1. 同一个shared_ptr对象可以被多线程同时读取。
2. 不同的shared_ptr对象可以被多线程同时修改。
3. 同一个shared_ptr对象不能被多线程直接修改，但可以通过原子函数完成。

如果把上面的表述中的"shared_ptr"替换为“内置类型”也完全成立。