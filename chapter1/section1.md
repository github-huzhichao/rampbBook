# 浅拷贝与深拷贝

再讲之前我们来解释下拷贝，顾名思义就是复制的意思。和物理上的拷贝不一样，在面向对象语言中拷贝涉及到类的继承、接口的实现等等。下面我们来讨论下浅拷贝与深拷贝的一些作用于区别。
拷贝：一定要有一个新对象的出现，并且这两个对象一定要相同。
下面是浅拷贝的一个例子：

```
class Person{
    public string name;
    public int age;
    public char gender;
    Random r;
    public Person(string name,int age,char gender){
        this.name=name;
        this.age=age;
        this.gender=r.Next(2)==1?'男':'女';
    }
}
Person p1=new Person("张三",18,'男');
//浅拷贝方法1：直接赋值拷贝创建对象
Person p2=new Person(p1.name,p1.age,p1.gender);
//浅拷贝方法2：使用类的初始化器创建对象
Person p3=new Person(){name=p1.name,age=p1.age,gender=p1.gender};
```

但是这种两种拷贝方法却又一个缺陷，就是当内部变量赋值出现不可预测的因素时（Person类中的性别就是不可控因素），不能保证拷贝后的对象与之前的对象一样。所有我们提出了一个新的解决方案如下：
思路：使用object类提供的一个受保护的方法来实现浅拷贝
①在对象中添加一个方法。
②该方法的返回值就是该类类型。
③return （该类类型）this.MemberwiseClone();
代码如下：

```
class Person{
public string name;
public int age;
public char gender;
public Person(string name,int age,char gender){
this.name=name;
this.age=age;
this.gender=gender;
}
public Person Clone(){
return (Person)this.MemberwiseClone();
}
}
Person p1=new Person("张三",18,'男');
Person p2=p1.Clone();
```

浅拷贝就不考虑对象中的引用类型成员，只是将对象中的内容全部拷贝一份，对于引用类型就是拷贝其存储的值，即地址。

而深拷贝就是把对象的所有成员都拷贝一份，包括引用类型成员。但考虑到类中的引用类型有可能有很多或者以后会添加新的引用类型。我这里直接介绍一种最直接的方法。
思路：通过对对象的序列化和反序列化来拷贝对象

```
class Person{
    public string name;
    public int age;
    public char gender;
    public Car MyCar;
    public Person(string name,int age,char gender,string carName){
        this.name=name;
        this.age=age;
        this.gender=gender;
        MyCar=new Car();
        MyCar.name=carName;
    }
    public Person Clone(){
        return (Person)this.MemberwiseClone();
    }
}
class Car{
    public string name；
}
//序列化对象
Person p1 = new Person("张三", 18, '男');
//创建二进制流对象
BinaryFormatter bf = new BinaryFormatter();
using (FileStream fs = new FileStream(@"E:\1.dat", FileMode.Create, FileAccess.Write)) {
    bf.Serialize(fs, p1);
}
//反序列化dat文件
//创建二进制流对象
BinaryFormatter bf1 = new BinaryFormatter();
//创建一个Persond对象来接收反序列化读的对象
Person p2;
using (FileStream fs1 = new FileStream(@"E:\1.dat", FileMode.Open, FileAccess.Read)) {
    p2 = (Person)bf1.Deserialize(fs1);
}
```