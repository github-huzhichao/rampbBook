# C#索引器

索引器在我们程序中的应用很普遍，那什么是索引器呢？


其实索引器就是一种特殊的类成员，它能够让对象以类似数组的方式来存取，使程序看起来更为直观，更容易编写。 在C#中的类成员可以是任意类型，
包括数组和集合。当一个类包含了数组和集合成员时，索引器将大大简化对数组或集合成员的存取操作。


定义索引器的方式与定义属性有些类似，其一般形式如下：
[修饰符] 数据类型 this[索引类型 index]
```
{
    get{}; //获得属性的代码
    set{}; //设置属性的代码
}
```

这里的数据类型是表示将要存取的数组或集合元素的类型。
索引器类型表示该索引器使用哪一类型的索引来存取数组或集合元素，可以是整数，可以是字符串；this表示操作本对象的数组或集合成员，
可以简单把它理解成索引器的名字，因此索引器不能具有用户定义的名称。

还可以通过索引器存取类的实例的数组成员，操作方法和数组相似，一般形式如：对象名[索引]
其中索引的数据类型必须与索引器的索引类型相同。

在接口中也可以声明索引器，接口索引器与类索引器的区别有两个：一是接口索引器不使用修饰符；二是接口索引
器只包含访问器get或set，没有实现语句。访问器的用途是指示索引器是可读写、只读还是只写的，如果是可读写
的，访问器get或set均不能省略；如果只读的，省略set访问器；如果是只写的，省略get访问器。

索引器与属性都是类的成员，语法上非常相似。索引器一般用在自定义的集合类中，通过使用索引器来操作集合对
象就如同使用数组一样简单；而属性可用于任何自定义类，它增强了类的字段成员的灵活性。