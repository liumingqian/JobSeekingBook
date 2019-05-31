# 设计模式

[《设计模式之禅》六个设计原则](https://blog.csdn.net/qq_24634505/article/details/80776964)

《设计模式之禅》23中设计模式

![](../.gitbook/assets/image%20%281%29.png)

\*\*\*\*

**单例模式**

**描述**： 构造函数私有化，避免被其他函数调用。客户端通过getInstance的方式调用。  
**优点**：避免对资源的多重占用，节约内存，可以设置全局访问点，优化共享资源访问。 **缺点**：扩展困难，可能与单一职责原则有冲突。 **使用场景**:要求对象必须唯一；整个项目需要一个共享访问点；创造对象消耗很多资源；需要定义大量静态常量和方法（也可以直接声明为static） **注意事项**：在高并发情况下应当注意单例模式的线程同步问题。如线程a和b同时执行，都判断到singleton==null，会使线程中同时存在多个对象。

**c\#实现**：

```text
    /// <summary>
    /// 单例模式的实现
    /// </summary>
    public class Singleton
    {
        private static Singleton instance;
        /// <summary>
        ///构造函数私有化
        /// </summary>
        private Singleton()
        {
        }

        /// <summary>
        /// 定义公有方法或属性提供全局访问点
        /// </summary>
        public static Singleton GetInstance()
        {
            if (instance == null)
            {
                instance = new Singleton();
            }
            return instance;
        }
    }
```

**c++实现**：

```text
Singleton* getInstance()
{
    if (instance == NULL)
        instance = new Singleton();

    return instance;
}
```

**抽象工厂模式**

**描述**：提供一个创建一系列相关或相互依赖对象的接口，只需要知道工厂方法，无需知道具体实现类。

**优点**：

**缺点**：

**使用场景**：系统有多个产品族，系统只消费其中某一族的产品。一个工厂定义多个同类产品。

**注意事项**：

**实现**：

### **Bridge模式\(接口与实现分离\)**

**描述**：①C++中的接口一般设计为不包含成员变量的抽象类，如果接口类中包含了成员变量，接口与实现就混合在了一起，会给派生带来麻烦（不管愿不愿意，子类必须接受父类的成员变量）。②声明一个和实现类接口完全相同的类，这样用指针引用该类的时候客户端不用重新编译，直接替换dll就行

**方法**：父类声明为只包含接口的抽象类，子类继承父类接口，并增加自己的接口，再用实现类继承子类，去实现子类接口

**优点**：抽象和实现之间不绑定，可以快速切换实现部分，不用把实现类细节提供给客户，修改实现部分时对客户端也无需重编译，不受影响。

**缺点**：每个实现类都要实现一遍接口的成员函数，比较繁琐。

**使用场景**： 隐藏抽象时间、共享实现或引用计数。

**注意事项**：

**实现**：

```cpp
//1 接口继承
class Shape {   // pure interface
public:
    virtual Point center() const = 0;
    virtual Color color() const = 0;
 
    virtual void rotate(int) = 0;
    virtual void move(Point p) = 0;
    // ...
};
 
class Circle : public Shape {   // pure interface
public:
    int radius() = 0;
    // ...
};

//2 实现
//实现放到了Impl这个名称空间中,这样使用Shape的客户端代码就不会随着Impl中Shape类的成员变量的变化而重新编译，
//因为客户端代码是看不到这些内容的，编译的时候也没有绑定这些信息。

class Impl::Shape : public Shape { // implementation
public:
    // constructors, destructor
    // ...
    virtual Point center() const { /* ... */ }
    virtual Color color() const { /* ... */ }
 
    virtual void rotate(int) { /* ... */ }
    virtual void move(Point p) { /* ... */ } 
    // ...
}

//3 考虑新增派生类的写法：
class Smiley : public Circle { // pure interface
public:
    // ...
};

class Impl::Smiley : Public Smiley, public Impl::Circle {   // implementation
public:
    // constructors, destructor
    // ...
```

### **工厂模式**

**优点**： 1、只需知道名称就可以创建一个对象

2、扩展性高，增加产品时只需增加一个工厂类 

 **使用场景**：不同条件下创建不同实例 

 **实现**：

```java
public class ShapeFactory {
    
   //使用 getShape 方法获取形状类型的对象
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }        
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      return null;
   }
}
```





**参考资料**：

* [http://www.runoob.com/design-pattern/design-pattern-tutorial.html](http://www.runoob.com/design-pattern/design-pattern-tutorial.html) RUNOOB设计模式教程
* 秦小波《设计模式之禅》
* [https://blog.csdn.net/fly542/article/details/6720217](https://blog.csdn.net/fly542/article/details/6720217) 设计模式----Bridge模式
* [https://blog.csdn.net/calmreason/article/details/53534766 ](https://blog.csdn.net/calmreason/article/details/53534766%20)C++设计：接口与实现分离

