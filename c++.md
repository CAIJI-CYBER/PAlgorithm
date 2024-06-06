# c++基本语法

## 杂项

```c++
1.值传递，地址传递，引用传递
2.new delete malloc free
3.对象指针
4.函数指针，返回指针的函数
5.指针数组
6.函数指针数组
7.子类对象赋值给父类对象
8.typedef（如何去定义类型，如何去定义函数）
 typedef oldname newname
9.auto关键字 和 decltype 关键字
10.const关键字
11.字节对齐
12.sizeof strlen
13.inline 和 宏定义
14.函数上 delete 和 default 用法
```









c++ 定义类和对象

```c++
class name{

    public:
      成员变量
      方法

}

创建对象
 int main(){
    name object1;  //创建一个类对象
}

三种权限： 默认是private
    public: 类内类外均可访问
    protected:类内可以访问，类外无法访问，子类可以访问。
    private:类内可以访问，类外和子类都无法访问。
```

```c++
c++的构造函数如果用户自己不写，那么编译器会默认分配空操作的构造函数和析构函数
    
 class name{
     name(){
         
     } //构造函数 ，可以有参数发生重载
     
     name(args a){
         
     }有参构造
     
     name(name &object){
         
     }拷贝构造
     
      name(): 成员1(值1),成员2(值2)
     {
         
     }   //初始化列表版无参构造器
         
     ~name(){
         
     }//析构函数，不能有参数
     
     
 }

int main(){
    name object: //不要加括号，不然是一个函数声明
    name object(args); //调用有参构造函数
    name object(name object)//调用拷贝构造函数
        
        
     name object=name(args) //另外一种方法   
}
```





```
类对象作为成员
class A{

}


class B{
  A a;
}


int main(){
  B b; 在创建B的对象时会先调用A的构造函数，再调用B的构造函数
}
```

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

```c++
this 指针用处：
    this指针是指向当前类的对象
   
    使用this来解决命名冲突
    
    使用this返回当前对象
```

```
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

```c++
运算符重载简单介绍 ：主要用于定义类的一些基本运算
下面简单介绍下 运算符重载方法
成员函数重载运算符和全局函数重载运算符（注意delete在重载时的作用）

例1：加号重载    
    class A{
        public :
          int m_a;
          int m_b;
          A operator + (A &p){
              A temp;
              temp.m_a=this.m_a+p->m_a;
              temp.m_b=this.m_b+p->m_b;
              return temp
          }   //使用成员函数进行运算符重载
    }

 A operator + (A &p1,A &p2){
      A temp;
      temp.m_a=p1->m_a+p2->m_a;
      temp.m_b=p1->m_b+p2->m_b;
      return temp
 }
 

 int main(){
     A a1;
     A a2;
     ......... //做些赋值操作
         
     A a3=a1+a2 //此时就会使用方法定义的方式去进行运算  其本质调用形势为opertor+(......);
 
 }

例2 : <<输出运算符重载 ,输出需要借助标准输出对象cout，所以必须使用全局重载
    
    class A {
        public:
         int ma;
         int mb;
    }
   void operator <<(ostream cout , A &p){
       cout << p->ma<<p->mb;
   }

例3: 赋值运算符
    class A{
        public:
         int  *num;
         void operator =(A &a){
             if(num!=null){
                delete num;
                 num=null;
             }
             num=*a.num
         }
        
    }

 例4：简单仿函数(在stl中用得比较多)
     class A{
         public:
          void operator()(String text){
              cout<< text<<endl;
          }
     }
  int main(){
      A a;
      a("hello")
       
       A()("hello") //匿名函数对象
  }
```

## 继承与多态简介

```c++
继承简单介绍
    class B{
        public :
          int num_b;
    }
    class A : public B{
         public:
        void 
    }


1.继承的写法：
    class 子类 ：继承方式 基类 {
        
    }

2.继承方式(三种继承方式是根据基类的成员继承到子类后的访问权限来划分的，在不适用友元的机制下，子类是无法访问基类中的私有类型的)：
    public ：基类中的非私有类型继承到子类中时访问权限不变
    protected : 基类中的非私有成员继承到子类时访问权限全部变为protected
    private ：基类中的非私有成员在继承到子类时访问权限全部变为private
        
        
3.继承时哪些东西会传下去	？
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
​          son son; //建立这个子类对象的时候，先调用基类的构造函数---> 调用子类的构造函数--->调用子类的析构函数--->调用基类的析构函数 
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
            fun(){
                
            }
 }
      
class son : public base{
 public:
          int var; //同名成员变量
          son(){
              var =1000;
          }
          ~son(){}
          fun(){
              
          } //同名的成员函数
}
    
int main()
{
 son son;
 cout<< son.var        // 访问子类中同名成员，直接访问即可
  cout<<son.base::var    // 访问基类中的同名成员，需要加上作用域

​    son.fun(); //这个会调用子类中的fun函数
​    son.base::fun() //这个会调用基类中的fun函数
​        
​    /*
​     注意如果由于子类中的同名成员函数会覆盖掉基类中的同名成员函数，所以在父类中出现了函数的重载时，需要注意编译器在调用时并不会因为参数而自动匹配到父类中重载的函数，所以也要加上作用域
​    */
}
​    
```



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
  虚函数表会存储虚函数的函数地址，子类重写父类中的虚函数时，子类的虚函数表将会覆盖父类的虚函数表将虚函数的函数地址替换成子类中重写的函数地址，没有重写或者没用虚函数则不会覆盖。
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

## 虚函数与虚函数表

```c++

```

## 类型转换

```c++

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



# STL简单使用

string的构造函数

![1656938856731](C:\Users\蒲照寰\AppData\Roaming\Typora\typora-user-images\1656938856731.png)

```
string 的输入
   string str
   cin>>str;
   getline(cin,str)
```

string中的find方法，用于发现子串

![1656939737769](C:\Users\蒲照寰\Desktop\背\1656939737769.png)

string::npos是如果没有包含关系则返回的东西。它的值是string对象能够存储的最大字符数，由此可以判断没有找到字串。

```
reverse函数
内存按照 n*16-1分配，
reverse(size t);
n*16-1>=t ；来算。
```



智能指针简单介绍

auto_ptr;   //这种在开发中比较少用，因为其有一定的缺陷，主要是如果定义两个这种类型的智能指针，将一个普通指针的值同时赋给这两个智能     指针，那么在两个对象析构时会发生错误。 例如：智能指针1=智能指针2 这样实际上这俩就引用的同一个地址。

unique_ptr;  //升级版1，同时赋值时会转让所有权。 

```
unique_ptr p1(new ........);
unique_ptr p2;
p2=p1; 所有权被剥夺，编译器会报错，那咋用这个指针。

一般是用于函数返回值，由于返回后，函数体内的是一个悬挂指针，就不会有危险了。 同理，匿名对象下的赋值也不会有问题。
如果你要硬赋值，可以使用        智能指针1=std::move（智能指针2）函数来。
```

shared_ptr;   //  升级版2，同一个对象地址被多个智能指针引用时，有一个计数器，析构一个智能指针，计数器就会-1，当且仅当计数器为0时才会delete

这三个本质上是三个模板类，new的对象的地址可以赋给他们，他们在生命周期结束的时候会析构，从而delete掉其中的内存，防止内存泄漏。

```c++
#include<memory.h> //智能指针的头文件
auto_ptr<double> pd(new double);
auto_ptr<string>   pd(new string);   //智能指针最基本的申请语法。其他两种智能指针的用法一致。
```



## 序列容器简单使用介绍

序列容器主要包含：array、vector、deque、string、stack、queue、priority_queues;









vector类简单使用：对标数组的容器

 

- ```c++
    创建（开拓什么类型的多大的容器）
    
    vector<double> vc(n) //n为多少个，可以是变量。
    
   使用下标访问vector中的元素。
     vc[0]=9;
     cout<<vc[0];
     
   在容器末尾添加元素：
     vc.push_back(11.0) ;
     
   迭代器：
   vector<double> iterator pd=vc.begin() //这是返回指向的头一个元素的指针。
   ++pd //遍历
   
   vector<double> iterator pd=vc.end() //指向最后一个元素后面一个的指针，用于作为停止标志
   
   删除和插入操作
   vc.erase(1,2)   //删除，两个参数为两个迭代器，第一个为删除元素的起始位置，第二个为删除元素终止位置加1；
   vc.insert(1,2,3) //插入，三个参数,第一个是被插入vector插入点的位置，元素会插入到这个位置前面，2是插入的元素组在另外一个vector中的起始位置，3是插入的元素组在另外一个vector中的结束位置（要插入的最后一个元素加1）
       
   遍历操作
    for_each(迭代器1.迭代器2，函数指针)   //对迭代器所指向的区间进行操作，操作的方式等于把作为参数传入函数指针。
    for(double num:vc){
        cout<<num;
    }  
   例如
    vectotr<double>::iterator pd;
    for(pd=vc.begin();pd!=vc.end();pd++){
         func(*pd);
    }
    等价于
    for_each(vc.begin();vc.end();func);
    
   随机排列操作
     random_shuffle(迭代器1,迭代器2);  //对迭代器区间内的元素进行随机分布
    
   排序操作
     sort(迭代器1.迭代器2); // 这样的排序如果是基本类型，直接进行即可，如果是自定义的类型，需要重写大小运算符<
   
  class A{
      public:
       int a;
  }
  
  bool operator<(const A &a1,const A &a2){
      if(a1.a>a2.a){
          return true;
      }
  }  //这种是升序排序
  
  另外一种方法
  sort(迭代器1.迭代器2，函数指针)  //该函数指针接受迭代器1和迭代器2作为参数，按照返回值为true方式进行排序。
  ```




## 关联容器简单使用介绍

set：键是唯一的，所以不能存储多个相同的值。



## 智能指针

### unique_ptr

```

```

### shared_ptr

```

```



#  c++ 基本IO简单介绍

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

## 信号与槽

```c++
1.建立连接的几种方式，及其区别
     connect(btn_close, SIGNAL(clicked()), this, SLOT(DoCloseSlot())); 传统写法，本质上是将函数名字转为字符串，参数不对应时编译器不会报错
    
    connect(this, &TestMoc::testMocSignal, this, &TestMoc::DoCloseSlot);
仿函数写法，参数不对应会编译出错
    
2.信号槽和boost库中的sianal的区别
3.第五个参数有什么用
AutoConnection： 自动连接 默认值 发送者和接收者同属一个线程下则为DirectConnection，否则为QueuedConnection
    
DirectConnection：直接连接，信号触发时先执行槽函数，再执行emit后的部分
QueuedConnection：队列连接，emit后的部分先执行，槽函数会在控制权回到控制者
BlockingQueuedConnection：
UniqueConnection：
4.emit函数
5.diconnect函数
```

## 事件循环流程

```c++

```

## delegate与mvcc(列表、表格、树)

```

```

## 元对象

```

```

## graphicview

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

## 多线程

```

```

## 网络

```

```

