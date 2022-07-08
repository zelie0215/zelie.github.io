# 什么是泛型编程

​	·忽略类型的一种编程 代码不会因为类型的改变而改变

~~~c++
//求最大值
int Max(int a,int b){
    return a>b?a:b;
}
string Max(string a,string b){
	return a>b?a:b;
}
double Max(double a,double b){
    return a>b?a:b;
}

//因为类型不同 就算函数体一样 要重新编写

//固定语法
template<class ty>
ty   Max(ty a,ty b){
    return a>b?a:b;
}





~~~





***



# 函数模板

 ## 函数中用到的位置类型

---



   ## 函数模板的调用


​	

~~~C++
template<class ty>
ty   Max(ty a,ty b){
    return a>b?a:b;
}
~~~



 ###   					隐式调用


~~~C++
//隐式调用
cout<<Max(1 , 2);  //ty = int;
cout<<Max(string("abc"),string("cab"))<<endl;  //ty = string
~~~

 ### 							显式调用	


~~~C++
//显式调用
cout<<Max<double>(1.11,2.22) <<endl;
~~~

---



 ## 		模板可以缺省

  - ~~~C++
    template <class ty1,class ty2=string>
        void printData(ty1 one,ty2 two)
    {
        cout<<one<<endl;
        cout<<two<<endl;
    }
    
    void func(){
        printData<int>(1,"张三");
    }
    ~~~

 ## 模板可以写变量

  - ~~~C++
    template <class ty=int, int size=0>    //ty = int,size=int
        ty* newMemeory(ty *p){
        return new ty[size];
    }
    void testconstSize(){
        //调用的时候必须要显式调用 并且只可以传入常量
        int *p  = newMemeory<int , 4>()
    }
    
    ~~~

 ## 一个类的成员也可以是函数模板

- ~~~c++
  class mm
  {
      public:
  		//函数模板
      	template <class ty>
              void print(ty a){
              cout<<a<<endl;
          }
  }
  ~~~

## 函数模板重载

### 			函数模板和普通函数

- ~~~c++
  template <class ty1,class ty2,class ty3>
  void printSum(ty1 one,ty2 two,ty3 three){
      cout<<"函数模板..."<<endl;
  	cout<<a<<endl;
      cout<<b<<endl;
      cout<<c<<endl;
  }
  
  void printSum(int one,int two,int three){
      cout<<"普通函数..."<<endl;
  	cout<<a<<endl;
      cout<<b<<endl;
      cout<<c<<endl;
  }
  
  printSum(1,2,3); //函数模板和普通函数 优先调用用匹配上的普通函数
  printSum<int,int,int>(1,2,3); //显式调用 百分百调用函数模板
  
  ~~~



****



# 类模板

### 		1.类用到了位置类型

### 		2.多文件中 类模板不能把定义声明分开实现

### 		3.类模板在调用的时候必须要显式调用 不用隐式调用

### 		4.类模板不是一个完整的类 所以在用到类名的地方 必须要用类名<类型>方式去使用

### 		5.类模板的特化处理	

### 		6.例子

~~~c++
template <class ty1,class ty2>
    class MM{
        public:
        	MM(ty1 one,ty2 two){
               one = one;
               two = two;
            }
        	void print();
    	protected:
        	ty1 one;
       		ty2 two;
    }
//用到类型的地方必须要用类名<类型> 的方式使用
template <class ty1.class ty2>	
	void MM<ty1,ty2>::print()
    {
        cout<<one<<endl;
        cout<<two<<endl;
    }

//实例化
void testMM(){
    MM<string,string> mm("abc","abc"); //类模板必须显式实例化
    MM<int ,string>* p = new MM<int ,string>(1,"张三");
	p->print();
    mm.pringt();
}

//继承
template <class ty1, class ty2>
    class girl ::public MM<ty1,ty2>{
        
        public:
        	girl();
    }

//函数特化
template <class ty1,class ty2,class ty3>
class Data{
    public:
    	Data(ty1 one,ty2 two,ty3 three): one(one),two(two),three(three){
            cout<<"原生模板"<<endl;
           
        }
    
    protected:
    	ty1 one;
    	ty2 two;
    	ty3 three;
}


//局部特化 特殊化处理
template<class ty1>
    class Data<ty1,ty1,ty1>{
            public:
    	Data(ty1 one,ty1 two,ty1 three): one(one),two(two),three(three){
            cout<<"局部特化"<<endl;
           
        }
    protected:
    	ty1 one;
    	ty1 two;
    	ty1 three;
}


//完全特化---> 针对特定的数据做特定的处理     两个Data类可同时存在
template()
class Data<int ,string,string>
{
    public:
   		Data(int one,string two,string) :one(one),two(two),three(three){
        	cout<<"完全特化" <<endl;
    	}
    protected:
    	int one;
    	string two;
    	string three;
    
}

//调用
void testcallclass(){
    Data<int,string,string> data1(1,"张三","王五");   //完全特化
    Data<string,string,string> data2("王五","张三","李四");  //局部特殊
    Data<string,int string> data3("王五",1001,"李四");		//原生模板
} 

//C++标准库 array 简单实现
template <class ty,int size>
    class Myarray{
        public:
        	Myarray()
            {
                memory = new ty[size]
            }
    	protected:
        	ty* memory;
    }

void testarray(){
    #include <array>
    array<int , 3> array1;
    Myarray<int , 3> Myarray2;
    
}




array<MM<int,string>,3> mmData;
//等效于
using DataType = MM<int ,string>;
array<DataType,3> mmData;

// :: 作用域分辨符

// 就近原则
age = 0;
void c(){
    age = 2;
    cout<<age<<endl;    // 2
    cout<<::age<<endl;  // 0
}

~~~

# 可变模板

### 	1.知道折叠参数类型：

#### 				1.类型是 ...

#### 				2.参数包 (变量名)   Args... arg

~~~c++
template <class ty>
    void printData(ty data){
    cout<<data<<" ";
}

template <class ... Args>
    void print(Args... args){
    int array[] = {(prntData(args),0)...};
}

void testprint(){
    print(1);
    print("adad",1,1.01);
}
~~~



~~~c++
//可变参数模板类 --> 特化 + 递归的方式先展开
//特化+继承的方式

tuple(string,string,int ,double) a;
tuple(int ,string) b;
//STL 容器 + 算法 + 迭代器

~~~



### 2,知道折叠参数的展开方式

