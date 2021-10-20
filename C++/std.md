定义于头文件<utility>

用于指示对象t可以被移动，即允许从t到另一对象的有效率的资源传递。特别是，std::move生成标识其参数t的亡值表达式，它准确地等价于到右值引用类型的static_cast。

可以非常简单的方式将左值引用转换为右值引用。

通过std::move，可以避免不必要的拷贝操作

视为性能而生，将对象的状态或者所有权从一个对象转移到另一个对象，只是转移，没有内存的搬迁或者内存拷贝。

```
template <typename T>
typename remove_reference<T>::type&& move(T&& t)
{
    return static_cast<typename remove_reference<T>::type&&>(t);
}
```