# 数据存储

## 变量初始化

c++中允许使用这种更简单的形式在声明变量时初始化，当类中定义了构造函数时也可以适用：

```c++
int i(5);

A::A(int x, int y)//类A的构造函数
{
    ...
}
A a(1,2);
```

## 强制类型转换

```c++
float c;
int a;
//两种格式都可以
c=float(a);
c=(float)a;
```

## 引用

- 引用(&)相当于为已有变量取别名，声明时必须初始化，且无法再改变。用关键字const限定时，被引用的对象不能被更新。只有用const限定时才可以引用一个常量。

```c++
int a;
int &b = a;//&符号仅起到标识作用
b = 10;//相当于a=10

const int &c = a;//无法通过c改变a的值
```

- 函数中使用引用变量作为形参时，表示调用函数时这个参数采用引用传递，函数内将能直接改动原数据。

```c++
void swap(int &a, int &b)//执行后a,b实现交换
{
	int t;
	t = a;
	a = b;
	b = t;
}
```

- 函数使用引用变量作为形参，但用关键字const修饰时，被引用的对象不能被更新。
- 函数使用引用变量作为返回值，好处在于不会在内存中产生返回值的副本，且函数返回后可以被赋值。此时返回对象不能是函数中的临时变量，而应该是静态变量、全局变量等函数返回后不会被销毁的数据。

```c++
int a;
int& func1()
{
    int b;
    //return b;不能返回临时变量
    return a；
}
func1() = 4;//引用变量作为返回值时可以被赋值

const int& func2()
{
    return a;
}
func2() = 4;//用const修饰的引用变量无法被赋值
```


## 动态内存分配

- 申请内存：new 类型名(初值列表)

  返回值为该类型的指针，分配失败时返回NULL

- 释放内存：delete 指针

```c++
int *p;
p = new int(2);//赋初始值为2
delete p;

p = new int[100];//申请一个int数组
delete p;
```

- 动态分配二维数组

```c++
//动态分配M*N维
int **a=new int *[M];
for(int i=0;i<M;i++)
  a[i]=new int[N];
//动态释放
for(int j=0;j<M;j++)
  delete[] a[i];
delete[] a;
```

# 函数

## 内联函数

将函数声明为内联函数(inline)时，编译后函数执行时会直接用语句直接替换，不会产生调用函数所需的开销。

```c++
inline void swap(int &a, int &b)
{
	int t;
	t = a;
	a = b;
	b = t;
}
```

## 默认参数

函数声明时可以给出默认形参的默认值，调用时没有给出实参则会使用这个默认值。

函数调用时会自动将实参从左到右分配到形参，因此默认形参值必须从右到左给出。

```c++
int add(int x, y=6)
{
	return x+y;
}

int main()
{
	add(10,20);//10+20
	add(10);//10+6
}
```

## 函数重载

函数重载(function overloading)指名称相同，但参数个数或类型不同的函数可以同时声明。const关键字也可用来区分重载函数，此时const对象只能调用const函数，非const对象优先调用非const函数。

```c++
//用参数类型或个数区分重载函数
int add(int a, int b);
double add(double x, double y);

//用const关键字区分重载函数
int add(int a, int b) const;
int add(int a, int b);
```

# 类

## 声明

```c++
class 类名称
{
	public:
		公有成员
	private:
		私有成员(只有本类对象可以访问)
	protected:
		保护型成员(只有本类及其派生类成员可以访问)
};
```

- 当未声明访问类型时，默认为private。(struct默认为public)

- 在类的声明中一般只给出函数原型，而函数体放置在类声明的外部。

- 函数体放置在类声明的内部时，表示该函数为内联函数。

- 函数体放置在类声明的外部时，需在函数名前加上类名进行限定。

```c++
class Clock
{
	public:
		void SetTime(int NewHour, int NewMin, int NewSec);
		void ShowTime()//内联函数
		{
			cout<<Hour<<":"<<Hminute<<":"<<Second;
		}
	private:
		int Hour, Minute, Second;
};
  
void Clock::SetTime(int NewHour, int NewMin, int NewSec)//函数名前加类名加以限制
{
	Hour=NewHour;
	Minute=NewMin;
	Second=NewSec;
}
```

## 前向引用声明

类似于函数原型的功能，类的前向引用声明可以使得在未给出类的完整定义时使用类。但是，只能使用被声明的符号，不能涉及类的任何内部细节，如定义该类的对象、使用该类的成员。

```c++
class A;//前向引用声明
void func(A a)//有前向引用声明，此处用法正确
{
	A aa;//错误，类没有完整定义，无法定义对象
}
```

## 指针

- this指针（自引用）：在类的内部存在一个隐含参数this，调用时指向该对象。在对象中使用该对象内的成员时无需添加对象名，自动隐含this指针。

```c++
void Clock::Function()
{
	Second++;
	//等同于
	//this->Second++;
}
```

## 构造函数

- constructor
- 用于对象创建时的初始化。
- 名字与类名相同。
- 没有返回值。(也不是void)
- 未声明时系统会自动产生。
- 允许内联、重载、默认形参值。

```c++
class Clock
{
    public:
    	Clock(int Hour, int Min, int Sec);//名称与类名相同
    ptivate:
    	int Hour, Minute, Second;
};

Clock::Clock(int Hour, int Min, int Sec)
{
    this->Hour = Hour;
    Minute = Min;
    Second = Sec;
}

int main()
{
    Clock MyClock(0,0,0);//调用构造函数
    return 0;
}
```

## 拷贝构造函数

- copy constructor
- 参数为指向该类对象的引用。
- 用于：利用已有对象初始化新声明的变量、调用函数时按值传递对象参数、返回值为类对象时。
- 编译器会自动生成拷贝构造函数，但可能是不正确的，例如存在指针的情况。
- 参数必须是引用类型的原因：当对象以值传递时，会默认调用拷贝构造函数用于生成副本，如果连拷贝构造函数都是值传递，会造成死循环。
- 参数用const的原因：并不一定要用const，但由于参数使用了引用，且函数的用途是复制，并不需要改变原对象，通常要使用const来避免不必要的修改。同时，使用const使得函数也可以使用常量变量作为参数。

```c++
Clock::Clock(const & C)//令参数为const类型，提高安全性
{
	Hour=C.Hour;
	Minute=C.Minute;
	Second=C.Second;
}

Clock a;
Clock b(a);//调用拷贝构造函数

void func1(Class C)//调用拷贝构造函数

void func2()
{
	Clock C;
	return C;//调用拷贝构造函数
}
```

## 组合类的构造函数

- 用于类的成员是另一个类的对象时对象的初始化。
- 调用后能同时完成本类中基本类型成员的初始化和对象成员的初始化。
- 和普通的构造函数基本相同，区别在于其函数体开头要单独描述各个对象成员的初始化方式。

```c++
声明格式：
类名::类名(参数表)
    :成员(初始化参数),成员(初始化参数),...
{
    本类初始化
}

class point
{
	float x,y;
};
class line
{
    point p1,p2;
    int width;
    ...
}

line::line(float x1, float y1, point p, int w)
    :p1(x1,y1),p2(p)//未列出的对象成员会使用默认构造函数
{
    width=w;
}
```

## 析构函数

- destructor
- 用于对象生存周期结束后自动完成善后工作，如释放动态申请的内存空间、关闭打开的文件。
- 系统一定条件下会自动生成。
- 函数名为类名前面加波浪号。

```c++
Class MyClass
{
	...
}

MyClass::MyClass()
{
	p = new int[100];
}

MyClass::~MyClass()
{
	delete p;
}
```

## 静态成员

- 静态数据成员：用关键字static声明，必须在类的外部定义和初始化，类的所有对象共用该成员的同一个拷贝。
- 静态成员函数：用关键字static声明，只能引用属于该类的静态数据成员或静态成员函数。
- 静态数据成员和静态成员函数在没有实例化对象时都可以直接调用。

```c++
class Point
{
	prirate:
		int x,y;
		static int count;//静态数据成员，用于记录一共生成了几个类的实例
	public:
		Point(int xx, int yy)
		{
			x=xx; y=yy;
			count++;
		}
		static int GetC()//静态成员函数
		{
			return count;
		}
};

int Point::count = 0;//静态数据成员的定义和初始化

int main()
{
    cout<<GetC()<<endl;//没有类的实例时静态成员函数也可以调用
}
```

## const修饰的对象成员

- 常成员函数：用const修饰(位于形参表之后)的成员函数，不能改变对象的数据成员，不能调用非常量的成员函数。是否有const关键字可用于区分重载函数。无论在函数声明还是函数体，const都不能被省略。
- 常数据成员：不能被修改的数据成员。

```c++
class R
{
	public:
		void print()；
		void print() const;
    private:
    	int R1,R2;
    	const int R3;
}
```

## 友元

- 友元函数：类外的函数，在类的声明中用关键字friend修饰说明，就可以访问该类的所有成员(包括private/protected)。声明属于private、public还是protected都无所谓。

```c++
class Point
{
	friend double Distance(Point a,Point b);//声明Distance函数是Point类的友元
	public:
		...
	private:
		int x,y;
};

double Distance(Point a,Point b)//函数Distance不属于类Point
{
	double dx=a.x-b.x;
	double dy=a.y-b.y;
	return sprt(dx*dx+dy*dy);
}
```

- 友元类：通过在类的声明中对另一个类进行友元声明，可以使另一个类的成员访问本类的所有成员。友元关系是单向的。

```c++
class A
{
	friend class B;//B可以访问A，但A不能访问B
	...
};
class B
{
	...
};
```

# 继承和派生

## 派生类的声明

```c++
//格式为在派生类名后添加：继承方式 基类名
class derived-class: access-specifier base-class
{
    ...
};
//可以同时继承多个基类，如：
class A: public B, private C
{
    ...
};
```

## 继承方式

基类中的private成员派生类中的任何成员都不可访问。但对于public、protected成员，继承方式可以改变派生类对象对基类成员的访问方式。

基类成员在不同的继承方式下在派生类中新的访问类型如下：

| 继承方式\基类成员    | public    | protected | private  |
| -------------------- | --------- | --------- | -------- |
| public继承（最常用） | public    | protected | 不可访问 |
| protected继承        | protected | protected | 不可访问 |
| private继承（缺省）  | private   | private   | 不可访问 |

- 基类中的私有成员，派生类的成员函数、派生类外部均不可访问。
- 派生类的成员函数可以访问基类中的public、private成员。
- 基类被继承后其public、protected成员是否还能被外界访问、被再次继承，由继承方式决定。

## 类型兼容规则

派生类是对基类的进一步细分，因此派生类属于基类，具有基类的特性。具体表现在；

- 派生类对象可以用来赋值、初始化基类对象。
- 基类对象的指针、引用可以指向派生类对象。
- 基类对象的指针、引用即使指向一个派生类对象，也只能访问基类的对象。

## 派生类的构造函数

基类的构造函数不会被继承，需要自己另外实现。但派生类的构造函数会默认调用基类的构造函数。

```c++
class A
{
public:
	A(int x);
	...
};
class B
{
public:
	B(int x);
	...
};
//C继承A、B
class C: public A, public B
{
public:
	C(int x, int y, int z);
private:
	int c;
}
//派生类的构造函数可以直接调用基类的构造函数
C::C(int x, int y, int z):
	A(x), B(y)//如果没有显式调用基类的构造函数，则会调用默认构造函数
{
    c = z;
}
```

调用顺序为先按继承时声明的顺序调用基类的构造函数，然后执行成员对象的构造函数，最后执行构造函数函数体中的内容。

## 派生类的析构函数

基类的析构函数同样不会被继承，但在派生类的析构函数执行结束后会自动隐式调用，因此不需要在派生类的析构函数中显式调用。

基类的析构函数应该定义为虚函数。

调用顺序与构造函数相反，先执行派生类中的成员，再执行成员对象，最后执行基类。

## 同名隐藏规则

基类中的成员，包括数据、函数，会被派生类中的同名成员覆盖。但是，可以通过基类名进行限定来访问基类成员。

# 多态

## 运算符重载

运算符重载本质上是将c++中的运算符视为函数，重载他们在运算对象不同时的实现。运算符重载可分为类成员函数、类外函数。要注意当运算符重载为类外函数时，要根据数据访问的需要将其设为类的友元函数。

```c++
class A
{
private:
    int x;
public:
	A operator-(A b);
    friend A operator+(A a, A b);
};
//类成员运算符重载隐含参数*this，如a-b相当于a.A::operator-(b)
A A::operator-(A b)
{
    A tmp;
    tmp.x = this->x - b.x;
    return tmp;
}
//类外运算符重载不隐含参数*this，a+b相当于operator+(a,b)
A operator+(A a, A b)
{
    A tmp;
	tmp.x = a.x + b.x;
    return tmp;
}
```

- 二元运算符（+、-等）：

```c++
//并不涉及返回值再次被赋值的问题，返回类型不需要是引用类型
A A::operator+(A b)
{
	A tmp;
	tmp.x = this->x + b.x;
	return x;
}
```

- 赋值运算符（=、+=、-=等）：要注意的是为了防止3=a这种语句的出现，编译器要求operator=必须是类内成员，且为了支持连续赋值的操作，赋值运算符的返回值应该是引用类型。

```c++
//c++中支持连续赋值，如a=b=c=1相当于分别执行c=1,b=c,a=b
//为了支持连续赋值的操作，赋值运算符重载的返回值应该是引用类型
class A
{
private:
    int x;
public:
    A& operator=(A b)
    {
        this->x = b.x;
        return *this;
    }
    A& operator+=(A b)
    {
        this->x = this->x + b.x;
        return *this;
    }
};
```

- 自增自减运算符（++、--）：要注意的是前置运算符和后置运算符使用一个参数int区分，该int仅起到区分的作用没有实际意义。同时为了和c++的自带类型相一致，最好前置运算符返回引用类型，后置运算符返回const类型。

```c++
//前置运算符和后置运算符的区别：
//a++表示a先参与运算再自增，++a表示a先自增再参与运算
class A
{
private:
    int x;
public:
    //前置运算符和后置运算符使用参数int区分
    A& operator++();//前置运算符，对应于++a
    const A operator++(int);//后置运算符，对应于a++
};
//c++中的自带类型，如int，允许++++a的语法，但不允许a++++的语法
//因此前置运算符应该返回引用类型支持连续自增，后置运算符应返回const类型拒绝连续自增
A& A::operator++()
{
    this->x++;
    return *this;
}
const A::operator++(int)
{
    A tmp = *this;
    this->x++;
    return tmp;
}
```

## 虚函数

虚函数的必要性在于：
- 支持派生类中重写基类中的同名同参数函数
- 支持动态联编：基类的指针、引用也可以指向派生类对象，静态联编时基类的指针只会调用基类的函数，而动态联编中基类的指针可以根据运行时实际指向的对象类型动态调用对应类中的函数

```c++
class phone
{
public:
    virtual int call();
    ...
};
//virtual关键字只在类声明中标识，函数实现时不能带有
int phone::call()
{
    ...
}
class mobile_phone: public phone
{
public:
    //只要基类中函数标识为了虚函数，派生类中的同名函数无论是否说明都自动为虚函数
    virtual int call();
    ...
};
```

- 为了析构函数的正确调用，有继承关系时析构函数必须是虚函数
- 构造函数不能是虚函数

## 纯虚函数

纯虚函数和虚函数的区别在于，派生类可以不重写虚函数，直接继承，而纯虚函数要求派生类将其重写。

```c++
class Base
{
public:
    //用virtual .. =0表示纯虚函数
    virtual void func() = 0;
    //纯虚函数可以有函数体，虽然终究会被重写，但是派生类的函数可以调用
    virtual ~Base() = 0;
}
//纯虚的析构函数必须有函数体，否则派生类的析构函数在自动调用时会出错
Base::~Base()
{
    ...
}
```

## 抽象类

- 一个类只要含有纯虚函数，那么它就是抽象类。
- 抽象类无法声明对象。
