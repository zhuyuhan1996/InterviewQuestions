用基本类型char实例化类模板basic_string，得到一个具体的模板类；

将得到的模板类实例typedef为string。因此，直观的理解为string是一个实例对象为char序列的模板类，自带了多封装好的针，可以实现对自身的操作，由于是模板类，因此可以很容易理解。

模板的三个参数介绍如下：

① charT 基本数据类型

② char_traits [1]

声明：template <class charT> struct char_traits;

作用：指定了字符的属性，并且提供了作用在字符或字符序列上的某些操作的特定语义。

③ allocator [2]分配器

声明：template <class T> class allocator;//<memory>头文件下allocator：

作用：定义用于标准库的部分内容，特别是STL的内存模型。