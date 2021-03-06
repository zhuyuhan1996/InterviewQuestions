\1) 封装性： C++的封装性就是将抽象类的函数和属性都封装起来，不对外开放，外部要使用这些属性和方法都必须通过一个具体实例对象去访问这些方法和属性，而我们知道， C 语言中一旦包含了头文件便可以使用头文件中的函数和变量，其实 C 语言中也可以用一种方法达到这种效果，那便是使用结构体+函数指针+static，结构体中定义属性和函数指针， static 将方法都限制在本模块使用，对外部，通过指针函数的方式访问，如此一来，便可以达到面向对象封装性的实现；

\2) 继承性： C++ 面向对象的继承是可以继承父类的属性和方法，在子类对象中的内存中是有父类对象的内存的，那么，用 C 语言来写的话我们完全可以在父类结构体中定义一个父类变量在其中，在使用构造子类的时候同时构造父类，便可以达到继承性的特性；

\3) 多态性： C++中允许一个父类指针指向子类实体，在这个指针使用方法时，若此方法是虚函数，则执行动作会执行到具体的子类函数中，本质的实现方式是通过一个虚函数指针的方式，由于我们用 C 语言写面向对象本就是通过函数指针的方式来封装函数，那我们完全可以将结构体父类的变量的函数指针让他指向子类的函数来达到多态的特性。