# c++基本语法

## 杂项

```c++
1.引用传递
 变量的名字实际为开辟内存的别名，引用实质上就是同一块内存的不同别名
2.malloc free new delete
  void* malloc(size_t size)
  :参数为开辟内存的大小，单位为字节
  :返回值为开辟内存区域的首地址，可进行强制转换，若开辟失败则返回空指针
  :开辟的空间不会初始化
  new[]和 delete[]搭配
  new 和 delete搭配，不然会报错
3.多维数组与多维指针
  可采用树状块图去分析
4.指针数组、数组指针
  指针数组就是存放指针的数组：element_type*  array[];
  数组指针就是指向数组的指针：可以看作多级指针的另外一种写法
  实例：
    int array1[10] : int *p;
    int array2[3][4] : int (*p)[4];
    int array[3][4][5]: int(*p)[4][5];
5.函数指针、指针函数
  函数指针是一个表示函数的指针：return_type (*f)(args,......);
  函数指针需要赋值后才能使用
  指针函数就是一个返回指针的函数
6.函数指针数组
  指的是装载函数指针的数组
  return_type (*f[size_t])(args,......)
7.typedef（如何去定义类型，如何去定义函数）  
  定义普通类型：typedef float f1;
  定义结构体：typedef struct{int a} INT;
  定义指针：typedef Node* node; //node为一个结构体类型，node为为此结构体类型定义的一个指针类型
  定义数组：typedef Node node[];
  定义数组指针类型：typedef element_type (*pArray)[5];
  定义函数指针：typedef return_type (*pFunc)(args,......)  
  复杂类型定义：typedef double (* (* (*fp3) ()) [10]) ();
如果创建许多复杂的定义，可以使用typedef。这一条显示typedef是如何缩短复杂的定义的。 跟前面一样，先找到变量名fp3（这里fp3其实是新类型名），往右看是圆括号，调转方向往左是*，说明fp3是一个指针；跳出圆括号，往右看是空参数列表，说明fp3是一个函数指针，接着往左是*号，说明该函数的返回值是一个指针；跳出第二层圆括号，往右是[]运算符，说明函数的返回值是一个数组指针，接着往左是*号，说明数组中包含的是指针；跳出第三层圆括号，往右是参数列表，说明数组中包含的是函数指针，这些函数没有参数，返回值类型是double。简言之，fp3是一个指向函数的指针，该函数无参数，且返回一个含有10个指向函数指针的数组的指针，这些函数不接受参数且返回double值。
8.数组作为参数
9.auto关键字 和 decltype 关键字
  auto必须马上初始化
  auto不能用于类的非静态成员变量
  auto不能定义数组
  decltype(exp) var;
  var的类型由exp决定
  ：exp如果是一个不被 ()包围的表达式，或者是一个单独变量、类成员访问表达式，那么var类型就和表达式类型一致
  ：exp是函数调用，则类型和返回值类型一致
  ：如果是一个左值或者被()包围，那就是exp类型的引用
10.const关键字
11.字节对齐
  ：结构体变量的首地址能够被字节对齐的大小整除(gcc 缺省字节对齐大小是4)
  ：结构体每个成员相对首地址的偏移需要是自身大小的整数倍
  : 结构体的总大小需要是结构体中最大类型大小的整数倍
12.sizeof 和 strlen区别
   sizeof是一个运算符、strlen是一个函数，参数是char*，作用是遍历直到'/0'的大小
   sizeof可以对很多种类型进行计算
13.inline 和 宏定义
  inline函数不能在循环中使用，会使代码膨胀
14.函数上 delete 和 default 用法
15.类的基本内存结构
16.右值引用简介
      
17.explicit关键字
18.动态库静态库区别
19.strlen和sizeof的区别
      前者运行期计算 后者编译期计算
      一个会退化一个不会
20.volatile关键字
      ：总是从所在的内存区域去读取
```

## 扫盲

```c++
public: 类内类外均可访问
protected:类内可以访问，类外无法访问，子类可以访问。
private:类内可以访问，类外和子类都无法访问。
final：
```

## 浅谈构造函数

```c++
class Obj
{
    //类中共有以下几种构造函数
    //默认构造函数：若没有手动定义任何构造函数，则编译器会默认提供一个
    //一般构造函数：程序员手动定义的一般构造函数一般是带各个字段的参数
    //转换构造函数
    //拷贝构造函数,自动调用时机如下（不考虑编译器优化）
    //当用类的一个对象去初始化该类的另一个对象时，系统会自动调用拷贝构造函数；
    //将一个对象作为实参传递给一个非引用类型的形参，系统会自动调用拷贝构造函数；
    //从一个返回类为非引用的函数返回一个对象时，系统会自动调用拷贝构造函数；
    //用花括号列表初始化一个数组的元素时，系统会自动调用拷贝构造函数。
    obj(const obj& objTarget);
    //移动构造函数，自动调用时机如下：
   /*非 const 右值引用只能操作右值，程序执行结果中产生的临时对象（例如函数返回值、lambda 表达式等）既无名称也无法获取其存储地址，所以属于右值。当类中同时包含拷贝构造函数和移动构造函数时，如果使用临时对象初始化当前类的对象，编译器会优先调用移动构造函数来完成此操作。只有当类中没有合适的移动构造函数时，编译器才会退而求其次，调用拷贝构造函数,若想用左值初始化时用移动构造，就可以使用std::move()*/
    obj(obj &&objTarget)
}
```

## 拷贝构造禁用

```c++
某些情况下我们封装一些类不希望这些类进行拷贝构造，因为有时复制会造成大量重复操作
介绍三种基本方法去禁用拷贝构造：
class obj
{obj(const obj& o)=delete} //显示禁用
class obj
{private:obj(const obj& o);}//定义为私有
class obj:public boost::noncopyable //使用三方库
{}
```

## 类对象和内部类

```c++
类对象作为成员
class A{
}
class B{
  A a;
}
int main(){
  B b; 在创建B的对象时会先调用A的构造函数，再调用B的构造函数
}

内部类：内部类可访问外部类的私有变量，但反过来不行
class Outter
{
public:
    void funcoutter()
    {
        Inner inner1;
        //inner1.inner;无权访问
    }
    class Inner
    {
    public:
        void funcinner()
        {
            Outter ot;
            ot.outter + 1;
        }
    private:
        double inner;
    };
private:
    double outter;
};


```

## 静态成员和静态成员方法

```c
静态成员和静态成员函数
class A{
public:
    static int  num;  类内声明，类外初始化
    static int getnum(){
   } //静态函数，只能访问静态变量
}
A::num=10;
int main(){
    A::getnum() //静态函数
}
```

## const成员

```c++
const修饰方法，则该方法为一个常函数，即其方法内不能修改非mutable成员变量
class a{
 public:
   int var;
   mutable  int var_m 
   void change () const
   {
       this.var=xxx //这种情况下会报错
       this.var_m=xxx //这种情况不会 报错
   }
}
const 修饰对象，则该对象为常对象，这种情况下也不能去修改非mutable成员变量的值，常对象只能调用常函数
```

## this指针

```c++
this 指针用处：
    this指针是指向当前类的对象
    使用this来解决命名冲突
    使用this返回当前对象
```

## 友元简介

```c++
友元
 三种实现方式
    1.全局函数做友元
   class a{
      friend void goodGay(a *a); //指定goodGay方法为友元方法，可以访问该类中的私有属性
       private:
         int private_var;
   }
   void goodGay(a *a){
       cout<<a -> private_var
   }
   int main(){
     a a;
      goodGay(&a)
   }
 2.友元类
    class a{
         friend class goodGay;  // 让goodGay类作为友元类，goodGay中的成员函数就可以访问 a中的私有成员
         private:
         int var;
    } 
  class goodGay{
    void test(a &a)
   }
  goodGay::test(a &a){
    cout<< a->var
    }
3.其他类的成员函数作为友元
    class Building{
        friend void  goodGay::visit(); //指定goodGay中的visit方法为友元方法
        public:
        Building();
         m_sittingroom;
         m_bedroom

         private:
          int var;
    }

    class goodGay{
        public:
         void visit();
         Building  * building;

    }
    void goodGay::visit(){
         cout << building->var;
    }  
```

## 继承简介

```c++
1.继承的写法：
    class 子类 ：继承方式 基类 {}
2.继承方式(三种继承方式是根据基类的成员继承到子类后的访问权限来划分的，在不适用友元的机制下，子类是无法访问基类中的私有类型的)：
    public ：基类中的非私有类型继承到子类中时访问权限不变
    protected : 基类中的非私有成员继承到子类时访问权限全部变为protected
    private ：基类中的非私有成员在继承到子类时访问权限全部变为private
3.继承时哪些东西会传下去    ？
        基类中的非静态成员变量全部会传给子类，其中私有成员即使访问不到，但还是会传给子类。
4.继承中的构造和析构问题
        class base{
            public:
             base(){

             }
            ~base(){

            }
        }


​      class son : public base{
​          son(){
​              
​          }
​          ~son(){
​              
​          }
​      }
​        
​      int main()
​      {
​          son son; //建立这个子类对象的时候，先调用基类的构造函数---> 调用子类的构造函数--->调用子类的析构函数--->调用基类的析构函数，这里是隐式调用，显示调用可以在构造函数中声明son():base(){...}
​      }

 5.继承中同名成员的问题
     即如果子类中有与基类中同名的成员，如何使用子类的对象去访问子类的中的改成员变量或者基类中成员，并且静态成员的的处理方式和非静态成员的处理方式一致。
 class base{
   public:
             int var;
             base(){
             var =100;
             }
            ~base(){}
            fun(){}
 }

class son : public base{
 public:
          int var; //同名成员变量
          son(){var =1000;}
          ~son(){}
          fun(){} //同名的成员函数
}

int main()
{
 son son;
 cout<< son.var        // 访问子类中同名成员，直接访问即可
 cout<<son.base::var    // 访问基类中的同名成员，需要加上作用域
 son.fun(); //这个会调用子类中的fun函数
 son.base::fun() //这个会调用基类中的fun函数       
/*
注意如果由于子类中的同名成员函数会覆盖掉基类中的同名成员函数，所以在父类中出现了函数的重载时，需要注意编译器在调用时并不会因为参数而自动匹配到父类中重载的函数，所以也要加上作用域
*/
}
final修饰类，则类无法被继承
final修饰虚函数，则函数无法被重写
```

## 虚拟继承

```c++
虚继承是为了解决菱形继承中基类的数据重复而诞生的
虚继承基类的构造需要在最终派生类构造函数中去进行
```

## 动多态简介

```c++
多态简单介绍
 分静态多态和动态多态两种
  函数重载和运算符重载这两个算是静态多态
  继承机制下的使用虚函数算动态多态

class animal{
    public:
     virtual void speak(){
         cout<<"animal";
      }    //虚函数
}


class cat : public cat{
    public:
      void speak(){   //这里可以加virtual 也可以不加
          cout<<"cat";
      }
}

void dospeak(animal &animal){
    animal.speak();
}
int main(){
    cat cat;
    dospeak(cat); //父类指针指向子类对象，实现多态，如果在基类中这个方法没有加上virtual关键字，则调用时会调用父类中的方法，而不会动态地去调子类的这个方法
}
/*
虚函数表：
  虚函数表会存储虚函数的函数地址，子类重写父类中的虚函数时，子类的虚函数表将会覆盖父类的虚函数表将虚函数的函数地址替换成子类中重写的函数地址，没有重写或者没用虚函数则不会覆盖
 */


/*
 由虚函数的应用特性来看，是由基类提供一个普遍的方法，各个子类进行各自特殊的实现，在应用时动态地去调用子类的实现，因此，在父类中的虚函数往往不需要写实现，此时可以将虚函数定义为纯虚函数，有包含纯虚函数的类称为抽象类，抽象类无法实例化对象，其子类要么实现父类中所有的纯虚函数，要么也成为抽象类。
*/
class base{
    virtual int f() =0; //纯虚函数，类为 抽象类，则该类无法实例化。
}

class son:public base{
    virtual int f(){
        cout<<"nihao";
    }
}

/*
虚析构和纯 虚析构
一个例子：
 class base{
    public:
    base(){
      ..........
    }
    ~base(){
        ............
    }
 }
 class son:public base{
      son(){

      } 
      ~son(){

      }
 }
int main(){
  base  *base=new son();//多态，父类指针指向子类对象
  delete base; //释放父类指针

 在这种情况下，不会调用子类的析构函数，这就会产生内存问题，所以产生了虚析构和纯虚析构，所以要让父类中的虚构为虚析构，从而去根据实际情况去调用子类中的虚构函数。
}
*/
```

## 基本运算符重载

### 赋值重载与移动语义

```c++
class A
{
	A& operator=(const A& right)
    {;}
    A& operator=(constA&& right);//移动赋值
}
```

### ()重载与仿函数

```

```

### *与[ ]运算符重载

```

```

### &运算符重载

## 虚函数表与RTTI

```c++
RTTI：
    typeid(A) 当A是如下几种类型时，将会进行类型推导
    ：一个指向多态类对象的指针的解引用
    ：一个指向多态类对象的引用 (只有存在虚函数才能触发多态)
```

## 类型转换

```c++
隐式转换和()强转都只是为了本次操作或运算而进行的临时转换，转换的结果也会保存到临时的内存空间内，不会改变数据本来的类型或者具体的值。
简单介绍下四种转换函数：
static_cast
:基本类型间的转换
:转换成void类型
:用于void*和其他类型指针间的相互转换,但不能用于两个不同类型指针的转换
:用于父子类对象或者指针的转换，向上转型安全，向下不安全
dynamic_cast
：只能适用于包含虚函数的基类和派生类之间的转换(指针或者引用)
：转换失败时若类型是指针则返回空指针，若是引用类型则抛bad_cast异常
：能进行向上向下和菱形继承下的横向转型
const_cast
: 需要特别注意的是 const_cast 不能去除变量的常量性，只能用来去除指向常数对象的指针或引用的常量性，且去除常量性的对象必须为指针或引用。
reinterpret_cast
:简单粗暴的进行字节位的拷贝，例如拿int去接一个指针会将地址值转为整数
```

## VOID* 简单介绍

```

```

## 深拷贝

```

```

## 泛型简介

```
模板编程
    cpp中有两种模板：函数模板、类模板

1.函数模板（一个通用的函数，其返回值或者参数列表或者函数体内的类型使用一个虚拟的类型）
    template <typename/class  T,.............................>//说明T为一个虚拟类型,可以设置多个虚拟类型
   函数定义

  例子：交换函数
 template <typename/class  T> 
 void  swap(T &num1, T &num2){
     T temp=num1;
     num1=num2;
     num2=T;
 }

int main(){
    //在调用写的函数模板时候有两种方式
    //1.自动类型推导，即调用时正常传入参数即可，编译器自动去推到类型
     int a=10;
     int b=20
    swap(10,20);
    // 2.显示指定类型
    swap<int> (a,b);
}

/*注意，自动类型推导下的函数模板无法发生类型的隐式类型转换，指定类型可以发生，普通函数会发生函数参数类型的隐式类型转换*/

2.普通函数和函数模板重名的情况
    如果普通函数和函数模板重名时候，优先调用普通函数。
    可以通过空模板参数列表的方式去调用去强制调用函数模板。
     void print(int a,int b){
        cout<<"normal";
      }

   template <typename T>
   void print(T a,T b){
       cout<<"template";    
   }

  int main(){
     print(1,2); //此时就会调用普通函数
     print<>(1,2) //此时使用了空模板参数列表，便可以强制去调用函数模板。

     print('c1','c2') //如果调用普通的时候要发生隐式类型转换时， 则会调用函数模板。
  }




3.类模板
    template <typename/class T,.................................>
    类定义;
   注意，类模板没有自动类型推导，并且类模板中可以使用默认参数

      例子：
        template<class Nametype,class Agetype=int> //此处使用了默认类型
         class person{
             public:
             person(Nametype name,Agetype age){
                 .......
             }
             Nametype name;
             Agetype age;
         }

​    int main(){
​         person<string,int>person(....)  //只能使用指定类型方式创建，没有自动类型推导。
​         person<string> person(....) //设定了默认类型情况下可以省略字段。
​    }

4.类模板中的成员函数的创建时机。
    class normal{
        public:
          void show(){

          }
    }

   template<class T>
   class tem{
       public:
        T &obj;
       test1(){
           obj.show();
       }
       test2(){
           obj.fuck() //子虚乌有的函数，但不会报错，因为不会创建他
       }
   }

  int main(){
      tem<normal> tem;
      tem.test1() //不报错
       tem.test2() //报错，因为调用时创建从而报错
  }

5.类模板对象作为函数参数传入
   template <class T1,class T2> 
   class person{
       public:
       person(T1 name,T2 age){
           ....
       }
       T1 name;
       T2 age;
   }

   void print1(person<string,int>) //指定传入类型；这种在开发中很常用

​    template <class T1,class T2>
​    void print2(person<T1,T2> p);  // 函数参数模板化，类似函数模板
​     
​    template <class T>
​     void print3(T &p)   //整个类模板化
​        
​        
6.模板与继承

template<class T>
class base{
    T m;
}

class son:public base<int> //必须要指定了基类模板的虚拟类型的具体类型才行
{

}  

要想灵活指定，子类也需要称为模板
 template<class T1>
 class son :public base<T1>{

 }

7. 类模板的成员函数的类外实现
   template<class T1,class T2>
     class P{
    public:
     P(T1 t1,T2 t2);
     }


  template<class T1,class T2>
  P<T1,T2>::P(T1 t1,T2 t2){
      ...............成员函数实现。
  }


8.模板的成员函数的分文件编写

 - person.h

   template<class T1>
   class person{
       public:
        person(T1 t1);
   }

-person.cpp


```



## 设计模式

### 原则

```
1.开放-封闭原则
    对扩展开放，对修改关闭
2.
```



### 单例

```c++
1.懒汉式基本实现
class singleton{
    public:
    static singleton* getInstance(){
        if(m_single==nullptr)
            m_single=new singleton();
        return m_single;        
    }
    ~singleton(){
        delete m_single;
    }
    private:
    singleton(){
    }
    singleton(singleton&)=delete;
    singleton& operator=(singleton&)=delete;
    static singleton* m_single;
};
缺陷：多线程下可能会产生多个实例

2.shared_ptr 加上锁模式
class Singleton{
public:
    typedef std::shared_ptr<Singleton> Ptr;
    ~Singleton(){
        std::cout<<"destructor called!"<<std::endl;
    }
    Singleton(Singleton&)=delete;
    Singleton& operator=(const Singleton&)=delete;
    static Ptr get_instance(){

        // "double checked lock"
        if(m_instance_ptr==nullptr){
            std::lock_guard<std::mutex> lk(m_mutex);
            if(m_instance_ptr == nullptr){
              m_instance_ptr = std::shared_ptr<Singleton>(new Singleton);
            }
        }
        return m_instance_ptr;
    }
private:
    Singleton(){
        std::cout<<"constructor called!"<<std::endl;
    }
    static Ptr m_instance_ptr;
    static std::mutex m_mutex;
};

3.局部静态变量下的单例
```

### 工厂模式

```
1.简单工厂
2.工厂模式
3.抽象工厂
```

### 观察者模式

```

```

### 代理模式

```

```

### 模板方法模式

```

```

### 策略模式

```

```

### 原型模式

```

```

### 责任链模式

```

```

### 简单反射

```

```

### 简单内存池

```

```

## STL简介

### 序列型

```
std::vector:
  capacity():当前分配的大小可容纳多少个元素
  size():当前已存数量 
  rbegin(),rend():反向迭代器
  扩容：开辟新空间、拷贝旧元素、释放旧空间
  正常扩容一般是倍数扩容，这样会将插入时间复杂度降低
  resize()是改变元素个数，只有超过capacity才会扩容，大于当前size会创建新元素，小于会删除
  reserve()是改变分配的大小，超过capacity会重新分配，小于不起作用
  emplace_back支持直接传入构造函数所需参数
```

### 键值对型

```
std::map:
std::unordered_map;
```

### 常见算法

```

```

### 智能指针

#### unique_ptr

```

```

#### shared_ptr

```
shared_ptr
{
	member* ptr;
    controlblock* ptr;
}
```

#### weak_ptr

```

```

#### auto_ptr

```

```



## c++ 基本IO简单介绍

cout和cin调整进制方法：  hex,oct,dec

输出刷新缓冲区

cout<<flush or flush(cout)

```
文件的IO
 文件输出流
   ofstream of; //定义输出流
   of.open("op.txt") //打开文件
     or

   ofstream of("op.txt")

   往文件里加东西
    of<<"sss";

  文件输入流
  ifstream if;
  if.open("op.txt");

  or 

  ifstream if("op.txt")


  if>>char[];

  关闭io流：io流.close();
```



# Qt基本使用

## 对象树

```c++
QT对象之间可以存在父子关系，每一个对象都可以保存它所有子对象的指针，每一个对象都有一个指向其父对象的指针。当指定QT对象的父对象时，父对象会在子对象链表中加入该对象的指针，该对象会保存指向其父对象的指针。当QT对象被销毁时，将自己从父对象的子对象链表中删除，将自己的子对象链表中的所有对象销毁。QT对象销毁时解除和父对象之间的父子关系，并销毁所有的子对象。
```

## 元对象

```C++
Q_OBJECT会激活元对象系统，会被赋予以下三机制
*信号槽机制
*元对象信息
    metaObject()方法可以返回QMetaObject;
    QMetaObject可获得以下信息
    类名和方法信息
*动态属性
    QMetaProperty
    QMetaClassInfo
    QMetaEnum
```

## 信号与槽

```c++
1.建立连接的几种方式，及其区别
     connect(btn_close, SIGNAL(clicked()), this, SLOT(DoCloseSlot())); 传统写法，本质上是将函数名字转为字符串，参数不对应时编译器不会报错
    connect(this, &TestMoc::testMocSignal, this, &TestMoc::DoCloseSlot);
仿函数写法，参数不对应会编译出错

2.信号槽和boost库中的sianal的区别

3.第五个参数有什么用
AutoConnection：  发送者和接收者同属一个线程下则为DirectConnection，否则为QueuedConnection
DirectConnection：直接连接，信号触发时先执行槽函数，再执行emit后的部分，该槽在信号线程中执行。
QueuedConnection：队列连接，emit后的部分先执行，槽函数会在控制权回到控制者线程后执行
BlockingQueuedConnection：使用Qt::BlockingQueuedConnection时，当信号被触发时，发送者会阻塞直到接收者处理完对应的槽函数，并且该槽函数会在接收者所属的线程中执行。这种连接类型适用于需要同步处理的情况，例如需要等待特定结果返回或完成某个操作后再继续执行。
UniqueConnection：唯一关联。这是一个标志，可使用按位或与上述任何连接类型组合。当设置 Qt :: UniqueConnection 时，则只有在不重复的情况下才会进行连接，如果已经存在重复连接(即，相同的信号指向同一对象上的完全相同的槽)，则连接将失败，此时将返回无效的 QMetaObject::Connection

4.diconnect函数
```

## 事件循环流程

```c++
主线程循环exec
事件发生被postevent送到队列 (sendevent是直接送到接收者)
分发器分发事件，Qapplication::notify送到接收者
若接收者有过滤器，则先走过滤器，eventFilter返回true则事件处理完不发给接收者了，反之则发。
事件接受者接受处理(可选择性处理，通用的还是调用基类处理方法，基类中对常见事件都做了默认处理，有需要可以重写)
若接受者
```

## delegate与mvc(列表、表格、树)

```C++
1.QModelIndex
2.QStandardItemModel
3.QVariant
4.QAbstractItemModel 和 Qdelegate
```

## graphicview与绘图

```

```

## 自定义控件

```

```

## 样式表

```

```

## 布局与位置控制详解

```

```

## 四大窗口

```

```

## MDI

```

```

## 多线程

```
1.继承QThread实现多线程
2.继承QObject并moveToThread实现多线程
3.使用线程池
4.QSemaphore信号量进行互斥
  acquire(n) ：请求n个信号量，若没有则阻塞
  release(n) ：释放n个信号量
5.QMutex
  lock()--unlock();手动调用
  QmutexLocker locker(mutex); 用locker上锁，作用域结束自动解锁
6.QWaitCondition使用
  wait(mutex):阻塞，并释放调mutex;
  waitone()
  waitall();
```

## 网络

```
    QSqlDatabase db = QSqlDatabase::addDatabase("QMYSQL"); // 或者 "QSQLITE", "QPSQL", 等
    db.setHostName("localhost");
    db.setDatabaseName("testdb");
    db.setUserName("root");
    db.setPassword("password");

    if (!db.open()) {
        qDebug() << "Failed to connect to database:" << db.lastError().text();
        return false;
    }
    return true;
   
   
   QSqlQuery query;
    if (!query.exec("SELECT * FROM test_table")) {
        qDebug() << "Query execution failed:" << query.lastError().text();
        return;
    }

    while (query.next()) {
        int id = query.value(0).toInt();
        QString name = query.value(1).toString();
        qDebug() << "ID:" << id << "Name:" << name;
    }
```

## 数据库

```sql

```

# 基本通信协议

## IP协议和MAC协议

## TCP协议

## UDP协议

```

```

# SQL基础

```

```

# 多线程基础

```

```

# Linux基本指令

```

```

# MAKEFILE

```

```

# CMAKE

```

```

# 数据结构

## 链表求环

```
快慢指针，两指针相遇时再释放一枚慢指针，两个慢指针相遇的点就是环的入口
```

## 栈求表达式

```

```

## 循环队列

```

```

## 冒泡

```
int currentButtom;
```

## 希尔排序

```

```

## 基数排序

```

```

## 堆排序

```

```

## 快速排序

```

```

## 归并排序

```

```

## kmp算法

```

```



## Hash查找简介

```
冲突时：
线性探测：往下一位找
平方探测：偏移平方去找
双散列：两个散列函数，第一个冲突了用第二个算
开链法：类似hashtable
```

## Prim算法

```

```

## Kruskal算法

```

```

## Dijstra算法

```

```

## 拓扑排序

```

```

## 关键路径

```

```

## 红黑树

```

```

## B+树

```

```

