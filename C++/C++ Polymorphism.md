# C++ - Polymorphism

Created by : Mr Dk.

2018 / 07 / 15 14:32

Hangzhou, Zhejiang, China

---

## Concepts

C++ 的多态性包含：

* 编译期多态：函数重载
* 运行期多态：虚函数重写

在函数调用时，到底使用哪个函数？编译器负责回答这个问题 - 将源代码中的函数调用解析为特定的函数代码块的过程被称为 **联编 (绑定)，binding**。C 语言中，每个函数名都对应了一个不同的函数，所以联编工作非常简单。C++ 中，由于需要支持多态性，联编工作变得复杂：

* 静态联编 / 早期联编 (static binding / early binding)：编译期间完成
* 动态联编 / 早期联编 (dynamic binding / late binding)：编译器生成能够在程序运行时选择正确虚函数的代码

## Pointer & References

公有继承使得指向基类的引用或指针可以指向派生类对象，而不必进行显式类型转换，即所谓 **向上强制转换 (upcasting)**。该转换关系不可逆。

## Virtual Method & Dynamic Binding

当基类指针指向派生类对象时，如果派生类对象的同名函数没有被声明为 `virtual`，则调用的是基类的函数；如果函数被声明为 `virtual`，那么编译器生成的代码将把函数调用关联到基类或派生类的同名函数上。

动态联编需要在 **运行时跟踪** 基类指针/引用指向的对象类型，增加了额外的开销。因此，静态联编的效率高，是 C++ 的默认选择。在继承关系中，有些函数不需要重新定义，那么就可以不用声明为 `virtual` 而降低性能。

编译器具体实现动态联编的方式：为每个对象添加一个隐藏成员 - 一个指向函数地址数组的指针 - **虚函数表 (virtual function table, vtbl)**。虚函数表中存储了类内声明为虚函数的函数地址。基类有一个虚函数表，派生类也有一个独立的虚函数表。

派生类的虚函数表被构造时，首先复制基类的虚函数表，然后将派生类内重定义的虚函数地址替换到派生类的虚函数表中，派生类内没有重定义的虚函数保持指向基类同名函数地址。如果派生类中提供了虚函数的定义，则追加该条目到派生类的虚函数表中。

调用虚函数时，程序通过 **运行时类型识别 (Runtime Type Identification, RTTI)** 获取到基类指针/引用指向的派生类对象的类型，然后根据派生类虚函数表中的地址调用虚函数。

> RTTI 使用 `dynamic_cast` 运算符进行安全的 downcast，转换成功将返回目标类型/引用类型，转换失败将返回 nullptr/bad_cast 异常。

虚函数的成本来自于：

* 每个对象将多一个成员变量 (指向虚函数表)
* 编译器要为每个类创建虚函数地址表
* 函数调用时，需要额外的查表操作

注意，构造函数不能是虚函数。因为基类与派生类的构造函数之间没有继承关系：先调用基类构造函数，再调用子类构造函数。将基类构造函数声明为 `virtual` 没什么实际意义。

而对于析构函数来说，如果不声明为虚函数，根据静态联编，基类指针将只会调用基类的析构函数，从而只回收部分派生类对象的内存。而当析构函数被声明为虚函数时，析构动态联编到派生类对象的析构函数上，然后再会调用基类的析构函数。

> 这意味着，当一个类需要被作为基类时，其析构函数必须是 `virtual` 的，否则将会产生内存泄漏。

---
