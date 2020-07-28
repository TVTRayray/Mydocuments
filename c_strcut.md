## 结构体 ##

``` c 
struct complex_struct {
double x , y;
} 
```

这样定义了complex_struct这个标识符，既然是标识符，那么它的命名规则就和变量一样，但它
不表示一个变量，而表示一个类型，这种标识符在C语言中称为Tag，struct complex_struct {
double x, y; }整个可以看作一个类型名，就像int或double一样，只不过它是一个复合类
型，如果用这个类型名来定义变如果用这个类型名来定义变量，可以这样写：

```c
struct complex_struct {
 double x, y;
} z1, z2;
```
这样z1和z2就是两个变量名，变量定义后面带个;号是我们早就习惯的。但即使像上面那样只定
义了complex_struct这个Tag而不定义变量，后面的;号也不能少。这点一定要注意，结构体定
义后面少;号是初学者很常犯的错误。不管是用上面两种形式的哪一种形式定义了complex_struct这个Tag，以后都可以直接用struct complex_struct来代替类型名了。例如
可以这样定义另外两个复数变量：

```c
struct complex_struct z3, z4;
```
如果在定义结构体类型的同时定义了变量，也可以不必写Tag，例如：
``` c
struct {
 double x, y;
} z1, z2;
```
但这样就没有办法再次引用这个结构体类型了，因为它没有名字。每个复数变量都有两个成员
（Member）x和y，可以用.运算符（.号，Period）来访问，这两个成员的存储空间是相邻
的[12]
，合在一起组成复数变量的存储空间。

结构体变量也可以在定义时初始化，例如：
```c
struct complex_struct z = { 3.0, 4.0 };
```
这样是错误的：
```c
struct complex_struct z1;
z1 = { 3.0, 4.0 };
```
结构体类型之间用赋值运算符是允许的，用一个结构体初始化另一个结构体也是允许的，例
如：

```c
struct complex_struct z1 = { 3.0, 4.0 };
struct complex_struct z2 = z1;
z1 = z2;
```
同样地，z2必须是局部变量才能用变量z1来初始化。

既然可以这样用，那么结构体可以当作函数的参数和返回值来传递就在意料之中了：

```c
struct complex_struct add_complex(struct complex_struct z1, struct
complex_struct z2)
{
 z1.x = z1.x + z2.x;
 z1.y = z1.y + z2.y;
 return z1;
}
```
***( complex_struct本身就是一种自定义的数据类型，当然也可以定义一个返回数据类型如此的函数。如上，定义了一个返回类型为complex_struct结构体的函数add_complex())***


这个函数实现了两个复数相加，如果在main函数中这样调用：

```c
struct complex_struct z = { 3.0, 4.0 };
z = add_complex(z, z);
```


 嵌套结构体
结构体也是一种递归定义：结构体由数据类型定义，因为结构体的成员具有数据类型，而数据
类型由结构体定义，因为结构体本身也是一种数据类型。换句话说，结构体也可以嵌套。例如
我们在复数的基础上定义复平面上的线段：

```c
struct Segment {
 struct complex_struct start;
 struct complex_struct end;
};
```
嵌套结构体可以嵌套地初始化。例如：
```c
struct Segment s = {{ 1.0, 2.0 }, { 4.0, 6.0 }};
```
也可以平坦地初始化。例如：
```c
struct Segment s = { 1.0, 2.0, 4.0, 6.0 };
```
甚至可以混合地初始化（这样可读性很差，应避免使用）：
```c
struct Segment s = {{ 1.0, 2.0 }, 4.0, 6.0 };
```
访问嵌套结构体的成员应该用多个.运算符，这也是意料之中的：
```c
s.start.t = RECTANGULAR;
s.start.a = 1.0;
s.start.b = 2.0;
```
例如下面实例的 [make_rational](###note1)





### refrenece

```c
#include<stdio.h>

//结构体定义
struct Rational {
int x;
int y;
};
```

```c
//初始化结构体
struct Rational make_rational(int x,int y)
{
	struct Rational r;
	r.x=x;
	r.y=y;
	return r;
};
```
### note1

```c
//打印出结构体
void print_rational(struct Rational r)
{
if (r.x==0) printf ("[+] 0\n");
else printf("[+] %d/%d\n",r.x,r.y);
}

struct Rational add_rational(struct Rational a,struct Rational b)
{
int result_x,flag,flag1;
if(b.y!=a.y)
{
	result_x=(a.x*b.y)+(a.y*b.x);
	
	if(result_x==0)
	{

		struct Rational r={0,0};
		return  r;
	}
	else
	{
		flag1=GCD(result_x,(b.y*a.y));
		struct Rational r ={result_x/flag1,(a.y*b.y)/flag1};
		return r;	
	}
}
else 
{
	result_x=a.x+b.x;
	if(result_x==0)
	{

		struct Rational r={0,0};
		return  r;
	}
	else
	{
		flag=GCD(result_x,b.y);
		struct Rational r={result_x/flag,b.y/flag};
		return r;	
	}

}


};


struct Rational sub_rational(struct Rational a,struct Rational b)
{
int result_x,flag,flag1;
if(b.y!=a.y)
{
	result_x=(a.x*b.y)-(a.y*b.x);
	
	if(result_x==0)
	{

		struct Rational r={0,0};
		return  r;
	}
	else
	{
		flag1=GCD(result_x,(b.y*a.y));
		struct Rational r ={result_x/flag1,(a.y*b.y)/flag1};
		return r;	
	}
}
else 
{
	result_x=a.x-b.x;
	if(result_x==0)
	{

		struct Rational r={0,0};
		return  r;
	}
	else
	{
		flag=GCD(result_x,b.y);
		struct Rational r={result_x/flag,b.y/flag};
		return r;	
	}

}


};



struct Rational mul_rational(struct Rational a,struct Rational b)
{

int result_x,result_y;
result_x=a.x*b.x;
result_y=a.y*b.y;

if(result_x==0)
{
	struct Rational r={0,0};
	return r;
}
else
{
	
	int flag;
	flag=GCD(result_x,result_y);
	struct Rational r={result_x/flag,result_y/flag};
	return r;
}


};


struct Rational div_rational(struct Rational a,struct Rational b)
{

int result_x,result_y;
result_x=a.x*b.y;
result_y=a.y*b.x;

if(result_x==0)
{
	struct Rational r={0,0};
	return r;
}
else
{
	
	int flag;
	flag=GCD(result_x,result_y);
	struct Rational r={result_x/flag,result_y/flag};
	return r;
}


};

//求最大公约数
int GCD(int a,int b)
{

if (a%b==0)  		return b;

else 			return GCD(b,a%b);

}

struct Rational guifan (struct Rational a,struct Rational b)
{
	
}
int main(void)
{
int x1,y1,x2,y2;
printf("please input x1/y1:\n");
scanf("%d,%d",&x1,&y1);
while(y1==0)
{
printf("[-] erro! the y can't be defined zero!\n");
printf("please input x1/y1:\n");
scanf("%d,%d",&x1,&y1);
}
struct Rational a=make_rational(x1,y1);
printf("please input x2/y2:\n");
scanf("%d,%d",&x2,&y2);

while(y2==0)
{
printf("[-] erro! the y can't be defined zero!\n");
printf("please input x2/y2:\n");	
scanf("%d,%d",&x2,&y2);
}
struct Rational b=make_rational(x2,y2);
printf("the result of add is:\n");
print_rational(add_rational(a,b));
printf("the result of sub is:\n");
print_rational(sub_rational(a,b));
printf("the result of mul is:\n");
print_rational(mul_rational(a,b));
printf("the result of div is:\n");
print_rational(div_rational(a,b));
}

```

