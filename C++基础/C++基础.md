## C++如何运行
1. 处理预处理，所有#开头的语句就是preprocess statement，编译器收到第一个源文件的时候第一件事就是处理预处理指令。也就是说编译前处理预编译指令。
	`# include <iostream> //把iostream文件里所有东西复制黏贴到当前文件`
	`header file 头文件`
2.  .cpp file will be compiled as a .obj file for windows
3. linker(链接器)把n个obj file链接在一起形成一个.exe
4. 通过output窗口看bug  
5. declaration 给compiler一个symbol, 告诉它存在这样一个东西
6. definition 定义这个函数的主体
7. 所以说编译一个包含存在在另一个源文件里的函数，只要在自身文件里声明，编译器就会相信
8. 是通过Linker去找到某个函数的definationr然后把它和当前的调用联系在一起。如果linker没找到，就会出现一个linking error
9.  that means a linker's job is resolving symbols
## 编译 compiling
1. preprocessing
2. tokenizing & parsing 标记解释&解析，也就是把英语翻译为机器理解的语言，创建 abstract syntax tree 抽象语法树
3. 说到底编译器的工作就是把代码转化为constant data 或者instruction
4. translation unit 和单个cpp文件不等价，如果一个cpp file include 很多其他的cpp file 那编译后也只有一个obj 
5. 关于预编译阶段的include 所做的一切就是把所include的东西复制到这个文件中
6. define的用处就是，搜索前面的词并把它们替换为后面的
7. if 依据特定条件包含或者剔除代码 if 0 会禁用这个区域中的代码
8. obj 文件是二进制的   
9. 编译优化的一些选项可以缩减编译后的代码长度 
10. 编译后的函数会带有特殊的函数签名，因为文件多了后，一个函数会出现在多个obj中，linker会根据函数签名进行工作
11. 调用某个函数的时候编译器就会生成一个Call指令
## linker
1. static函数的意义就是
2. 两个同名同签名函数在一个编译单元时，会产生编译错误；如果不在 一个编译单元，build的时候LK错误
3. 在不同cpp中include同一个头文件会产生重复符号错误，因为预编译阶段会两次复制并编译这段函数
4. 上述问题的解决方法是头文件里的函数用static，这意味着被include 的函数只在这个translation unit 内部有效
5. 另一个方法就是把函数的种类变为inline函数，这样的话调用的时候，相当于只把内容复制过来进行直接使用
6. 还有就是头文件只申明函数，不实现函数
## header
1. 头文件中的pragma once 是为了防止多次include某个头文件到同一个编译单元
## Debug
 1. break point
 2. read memory  
## Condition Statement
1. 一个优化方式叫做 constant folding
2. 查看disassemblely 
## Set up
1. source 或者src文件夹下面放源代码
2. 默认的结构是假象，只是虚拟的filter而不是folder
	![[Pasted image 20240510190217.png]]
## Raw Pointer
1. 事实上，pointer是一个int,储存了一个整数，它储存了一个内存的地址
```
int var=8;
int* ptr=&var; //int* tell the complier this is a int 这样反取址的时候，就知道这个指针指向的东西是什么，应该有多大的内存
*ptr=10 //访问ptr所指向的数据的内存(读写这块内存中的数据)
char* buffer=new char[8];//分配了八个字节的内存，并返回一个指向这块内存的开始地址的指针
memset(buffer,0,8); //用0填充指针buffer开始的8个子节
char** ptr=&buffer

delete[] buffer; //new的东西在heap上
```
## Reference
引用必须引用一个已经存在的变量，引用本身并不是一个新的变量。
```
	int a=5;
	int* b=&a;
	
	//敲重点
	int& ref=a; //int& 作为整体是一种类型。它其实不是变量，它是引用
	ref=2; //这样a也会变成2，we just crete an alias for a
	//如果有下面这样一个函数
	void Increment(int value)
	{
		value++;
	}
	Increment(a); //这时只是令value等于5，实际上a并没有增加
	
	//如果我想增加的是a的本体应该怎么办？可以将a的地址传给函数，而不是把数值传给函数
	void Increment2(int* value)
	{
		(*value)++;
	}
	Increment(&a);
	
	//实现以上功能，用引用或许更加便捷
	void Increment3(int& value) //传参的过程等于赋值，所以这一步value将等同于a
	{
		value++;
	}
	//once you declare a reference, you cannot change what it references
```
## Classes in Cpp
```
class Player
{
public:
	int x,y;
	int speed;
	void Move(int xa,ya)
	{
		x+=xa*speed;
		y+=ya*speed;
	}
};
```
### Difference between struct
Class is private by default, struct is public by default.
struct的存在主要是为了保证cpp和C的兼容性
struct----plain old data (pod)
sth just represent variables can use struct and never use inheritance in struct

## Define & Declaration
- **声明（Declaration）**：告诉编译器某个变量或函数的存在以及其类型，但不分配内存或提供具体实现。例如，`extern int x;` 是对变量 `x` 的声明，表明它在其他地方定义；`void foo();` 是对函数 `foo` 的声明，表明它的存在，但没有具体实现。
- **定义（Definition）**：提供了变量或函数的具体实现，并分配内存。例如，`int x = 5;` 是变量 `x` 的定义，同时也为它分配了内存；`void foo() { /* 实现代码 */ }` 是函数 `foo` 的定义，提供了其具体的功能实现。

## Static
### outside
类外的static修饰符号在link阶段是局部的，也就是它只对定义它的编译单元可见
类或结构体里面的static表示这部分内存是这个类的所有实例共享的。对于方法也是一样，静态方法里没有该实例的指针
C++中，两个全局变量不能名字一样，否则会报link错误，也就是此变量已经在另一个地方被定义了
在一个地方定义了一个变量，另一个文件中extern这个变量，那么Linker就会在另外的编译单元里寻找这个变量的定义。如果此时，定义它时加上了static关键字，那么linker还是找不到它，因为static 在link阶段只对当前编译单元可见
如果在hpp里声明了static函数，且这个hpp被两个cpp include，那实际上就是这个static 函数在不同的编译单元中出现了，这样也不会出现问题，因为互相不可见
### inside the class and struct
这种情况下的静态变量需要单独定义
```
struct Entity
{
	static int x, y;
	void Print()
	{
		std::cout<<x<<","<<y<<std::ednl
	}
}
int Entity::x;
int Entity::y;
int main()
{
	Entity e;
	e.x=3;
	e.y=5;
	//也可以不用实例的指针引用静态变量
	Entity::x=5;
	Entity::y=8;
}
//只要改变一个实例中的x,y所有实例中获取的x,y都会变
```
如果类内的函数是静态的，但是函数中涉及到的变量是非静态的，事情就不一样起来，**因为静态方法不能访问非静态变量**，原因是静态方法没有类实例。本质上，你在类里写的每个非静态方法都会获得当前的类实例作为参数（this 指针），然而静态方法不知道怎么访问非静态变量，因为你没有给他一个实例的引用
## Local Static
```
//第一次调用函数时创建变量，并被设置为0
void Function()
{
//静态变量i只在函数作用域中起效果，函数外不能访问到
	static int i=0;
	i++;
	std::cout<<i<<std::endl;
}
int main()
{
//这时输出的i会递增
	Function();
	Function();
	Function();
}
//单例类
class Singleton
{
private:
	static Singleton* s_Instance;
public:
	static Singleton& Get() {return *s_Instance;}
	void Hello() {}
}
//还需要在外部声明这个实例
Singleton*Singleton::s_Instance=nullptr;
int main()
{
//get返回的是Singleton的实例
Singleton::Get().Hello();
}

//可以删除实例的外部定义的写法
class Singleton
{
public:
	static Singleton& Get() {
	static Singleton instance;
	return instance;}
	void Hello() {}
}
```
## Summary for Static
static variables exist for the life time of the translation unity that it defined in,
	 If it's in a namespace scope (i.e. outside of functions and classes), then it can't be accessed from any other translation unit. This is known as "internal linkage" or "static storage duration". (Don't do this in headers except for `constexpr`. Anything else, and you end up with a separate variable in each translation unit, which is crazy confusing)
	If it's a variable _in a function_, it can't be accessed from outside of the function, just like any other local variable. (this is the local they mentioned)
	class members have no restricted scope due to `static`, but can be addressed from the class as well as an instance (like `std::string::npos`). [Note: you can _declare_ static members in a class, but they should usually still be _defined_ in a translation unit (cpp file), and as such, there's only one per class]
```
static std::string namespaceScope = "Hello";
void foo() {
    static std::string functionScope= "World";
}
struct A {
   static std::string classScope = "!";
};
```

### Variables

`static` variables exist for the "lifetime" of the _translation unit that it's defined in_, and:

- If it's in a namespace scope (i.e. outside of functions and classes), then it can't be accessed from any other translation unit. This is known as "internal linkage" or "static storage duration". (Don't do this in headers except for `constexpr`. Anything else, and you end up with a separate variable in each translation unit, which is crazy confusing)
- If it's a variable _in a function_, it can't be accessed from outside of the function, just like any other local variable. (this is the local they mentioned)
- class members have no restricted scope due to `static`, but can be addressed from the class as well as an instance (like `std::string::npos`). [Note: you can _declare_ static members in a class, but they should usually still be _defined_ in a translation unit (cpp file), and as such, there's only one per class]

locations as code:

```cpp
static std::string namespaceScope = "Hello";
void foo() {
    static std::string functionScope= "World";
}
struct A {
   static std::string classScope = "!";
};
```

Before any function in a translation unit is executed (possibly after `main` began execution), the variables with static storage duration (namespace scope) in that translation unit will be "constant initialized" (to `constexpr` where possible, or zero otherwise), and then non-locals are "dynamically initialized" properly _in the order they are defined in the translation unit_ (for things like `std::string="HI";` that aren't `constexpr`). Finally, function-local statics will be initialized the first time execution "reaches" the line where they are declared. All `static` variables all destroyed in the reverse order of initialization.

The easiest way to get all this right is to make all static variables that are not `constexpr` initialized into function static locals, which makes sure all of your statics/globals are initialized properly when you try to use them no matter what, thus preventing the [static initialization order fiasco](https://stackoverflow.com/questions/3035422/static-initialization-order-fiasco).

```cpp
T& get_global() {
    static T global = initial_value();
    return global;
}
```

Be careful, because when the spec says namespace-scope variables have "static storage duration" by default, they mean the "lifetime of the translation unit" bit, but that does _not_ mean it can't be accessed outside of the file.

### Functions

Significantly more straightforward, `static` is often used as a class member function, and only very rarely used for a free-standing function.

A static member function differs from a regular member function in that it can be called without an instance of a class, and since it has no instance, it cannot access non-static members of the class. Static variables are useful when you want to have a function for a class that definitely absolutely does not refer to any instance members, or for managing `static` member variables.

```cpp
struct A {
    A() {++A_count;}
    A(const A&) {++A_count;}
    A(A&&) {++A_count;}
    ~A() {--A_count;}

    static int get_count() {return A_count;}
private:
    static int A_count;
}

int main() {
    A var;

    int c0 = var.get_count(); //some compilers give a warning, but it's ok.
    int c1 = A::get_count(); //normal way
}
```

A `static` free-function means that the function will not be referred to by any other translation unit, and thus the linker can ignore it entirely. This has a small number of purposes:

- Can be used in a cpp file to guarantee that the function is never used from any other file.
- Can be put in a header and every file will have it's own copy of the function. Not useful, since inline does pretty much the same thing.
- Speeds up link time by reducing work
- Can put a function with the same name in each translation unit, and they can all do different things. For instance, you could put a `static void log(const char*) {}` in each cpp file, and they could each all log in a different way.
- 
### 静态局部变量（**Static Local Variables）

当 `static` 用于函数内部的变量时，它使变量具有静态存储期，这意味着变量在整个程序运行期间保持其值，而不是每次函数调用时都重新初始化。也就是说，这种变量在第一次调用函数时被初始化，之后的调用会使用它上次保存的值。
### **静态成员变量（Static Member Variables）**

在类中，`static` 用于成员变量时，意味着该变量属于类而不是类的任何对象。这意味着所有类的对象共享这个变量，而不是每个对象有自己独立的副本。
### **静态成员函数（Static Member Functions）**

静态成员函数也属于类本身，而不是类的任何特定对象。它可以访问静态成员变量和调用其他静态成员函数，但不能访问非静态成员变量和成员函数。
### **静态全局变量和函数（Static Global Variables and Functions）**

在源文件（`.cpp` 文件）中，`static` 关键字用于全局变量或函数时，限制它们的作用域仅限于定义它们的文件。这意味着它们对其他文件不可见，从而避免了命名冲突。
## Enum
```
enum Example:unsigned char
{
	A=5,B,C
};
```
C++中的`enum`（枚举）类型只能是整数类型，因为枚举的设计目的是用来表示一组命名的整数常量。浮点数无法作为枚举类型，因为浮点数的表示和运算方式与整数不同，涉及精度问题和不同的存储格式，使得枚举的简洁和高效特性难以实现。如果你需要使用浮点数，可以考虑使用`const`变量或`constexpr`变量来代替。
## Constructor
In cpp you must initialize all by yourself
if you use the static method of the class without instance, constructor will not work
set the constructor to private, then cpp cannot run constructor automatically.
## Destructor
~Entity(){ }
栈上的内存会自动释放
### Inheritance

## Virtual Function
virtual function let us overwrite method in sub class
if we created a func in class A and mark it as a virtual func, we can overwrite the func in class B let the func do sth else.
```
class  Entity
{
public:
virtual std::string GetName()
{
	return "Entity";
}
}

class Player:public Entity
{
	private:
		std::string m_Name;
	public:
	Player(const std::string& name):m_Name(name){}

std::string GetName() override {return m_Name;}
}
int main()
{
	Entity* e=new Entity();
	std::cout<<e->GetName()<<std::endl;
	Player*p=new Player("Hilda");
	std::cout<<p->GetName()<<std::endl;
	std::cin.get();
}
```
## Interface in C++(Pure virtual func)
纯虚函数允许我们定义一个在基类中没有实现的函数，强制子类实现这个函数。接口就是一个只包含未实现的方法并作为一个模板的类。由于此接口类实际上不包含方法的视线，所以我们就无法实例化这个类 
```
virtual std::string GetName()=0;
```
所以只能实例化一个实现了所有纯虚函数的类
子类不override父类中的虚函数的话就执行父类的
## Visibility
private, public, protected
## Array
为什么有时要 new在堆上，而不是直接用栈？
	主要是因为生存期的问题，new分配的内存会一直存在，直到你delete他
	如果你有个函数要返回新创建的数组，那么必须要用new来分配他，除非你传入的参数本来就是一个内存地址
memory indirection
	我们有一个指针，指针指向另一个保存我们实际数组的内存块
		p-> p-> array
C++中无法计算数组的实际大小
	对于栈上分配的 int a[5], 可以sizeof(a), =20
```
class Entity
{
public:
	static const int exampleSize=5;
	int example[exampleSize];//数值必须是编译时就直到的数，所以要加static,栈中为数组申请内存时，他必须是一个编译时就需要知道的常量

	Entity()
	{
		for(int i=0;i<5;i++)
			example[i]=2;
	}
}
```
constexpr是啥？？
		
## string & char
存疑，不确定，之后再看看

## Const & mutable
const int* = int const *
int * const
还可以用在方法名后面
mutable 的变量可以被const的方法改变
mutable还可能和lamda一起用
lamada存疑？之后用到再看看
## 构造函数初始化列表
不论初始化列表什么顺序，都会按照类里面变量的顺序进行初始化
为什么要用初始化列表
	因为初始化列表里可以用默认的参数，不用的话浪费
## Ternary operators
```C++
s_Speed=s_Level>5?10:5;
```
## how to create instantiate object
