---
title: C语言实现Complex
---

数据结构与算法--王卓 视频中的案例，实现一个复数类

之前学的 `C++` 发现和 `C` 还是有一定的区别

最主要的是 `C++` 具有面向对象的特性，可以将这个放在类里，类内有成员函数

而 `c` 不一样，只能使用结构体，这就显得没那么方便

## 一、代码实现

### 1、定义 complex 结构体

```c
typedef struct
{
	float realpart;  //实部
	float imagpart;	 //虚部
}Complex;
```

> typedef 简单来说就是起别名，给结构体起别名
>
> 假如定义一个Node结构体
>
> 如果是 **struct Node {};**
>
> 那么在声明 Node 变量的时候，就只能写成 struct Node n;
>
> 而如果是 **typedef struct Node {}N;** 或者是 **typedef struct {}N;**
>
> 那声明就可以用 N n;

### 2、赋值操作

```c
//赋值
void assign(Complex *c,float real,float imag)
{
	c->realpart = real;
	c->imagpart = imag;	
}
```

> 这里传入一个 Complex 的引用c，这样就可以修改c的值

### 3、相加操作

```c
//相加
Complex add(Complex a,Complex b)
{
	Complex c;
	c.realpart = a.realpart + b.realpart;
	c.imagpart = a.imagpart + b.imagpart;
	return c;
}
```

> 实部相加，虚部相加

### 4、相减操作

```c
//相减
Complex minus(Complex a,Complex b)
{
	Complex c;
	c.realpart = a.realpart - b.realpart;
	c.imagpart = a.imagpart - b.imagpart;
	return c;
}
```

> 和相加同理

### 5、相乘操作

```c
//相乘
Complex multiply(Complex a,Complex b)
{
	Complex c;
	c.realpart = a.realpart * b.realpart - a.imagpart * b.imagpart;  
	c.imagpart = a.realpart * b.imagpart + a.imagpart * b.realpart;
	return c;
}
```

> 相乘拆开，两项含 **i** 的相乘变为虚部的相反数

### 6、相除操作

```c
//相除
Complex divide(Complex a,Complex b)
{
	Complex c,d,numerator;
	int denominator;
	float mul; //分母的倒数，用于乘以实部和虚部，得到最终结果
	assign(&d,b.realpart,-b.imagpart);
	//分子
	numerator = multiply(a,d);
	//分母
	denominator = b.realpart * b.realpart + b.imagpart * b.imagpart;
	mul = 1/(float)denominator;
	c.realpart = mul * numerator.realpart;
	c.imagpart = mul * numerator.imagpart;
	return c;
}
```

> 相除相对复杂
>
> 首先进行分母有理化
>
> 再先把分母提出来，分子要同时乘以分母有理化时乘以的复数
>
> 得到的分子是一个复数
>
> 再乘以分母的倒数
>
> 最终得到相除后的复数

### 7、打印操作

```c
//打印
void printComplex(Complex c)
{
	printf("复数的实部为: %g ,虚部为: %g\n",c.realpart,c.imagpart);
	printf("复数表示成: %g + %gi",c.realpart,c.imagpart);
}
```

> %g 和 %f 的区别
>
> %g , 只显示有效数字，会省略后面的0
>
> %f, 则不会省略

## 二、测试

``` c
int main(void)
{
	Complex com1,com2,com3;
	assign(&com1,8,6);
	assign(&com2,4,3);
	com3 = divide(multiply(com1,com2),add(com1,com2));
	printComplex(com3);
	return 0;
}
```

> 用的是，8 + 6i 和 4 + 3i 相乘，并处以二者的和
>
> 得到结果与验算结果一致，测试通过