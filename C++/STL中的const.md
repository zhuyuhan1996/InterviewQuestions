声明迭代器为const就像声明指针为const一样，表示这个迭代器不得指向不同的东西，但是它所指的东西的指是可以改变的。

const vector<int>::iterator iter=vec.begin();++iter是不行的，

vector<int>::const_iterator iter=vec.begin();*iter是不行的。

const实施于成员函数的目的，为了确认该成员函数可以作用域const对象上

第一：它们使class接口比较容易被理解

第二：它们使操作const对象成为可能。

mutable释放掉non-static成员变量的const成员函数的约束。