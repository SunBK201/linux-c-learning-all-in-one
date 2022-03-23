# Page 2

`if`语句还可以带一个`else`子句（Clause），例如：

```
if (x % 2 == 0)
	printf("x is even.\n");
else
	printf("x is odd.\n");
```

这里的%是取模（Modulo）运算符，`x%2`表示`x`除以2所得的余数（Remainder），C语言规定%运算符的两个操作数必须是整型的。两个正数相除取余数很好理解，如果操作数中有负数，结果应该是正是负呢？C99规定，如果`a`和`b`是整型，`b`不等于0，则表达式`(a/b)*b+a%b`的值总是等于`a`，再结合[第 5 节 “表达式”](https://akaedu.github.io/book/expr.expression.html)讲过的整数除法运算要Truncate Toward Zero，可以得到一个结论：_%运算符的结果总是与被除数同号_（想一想为什么）。其它编程语言对取模运算的规定各不相同，也有规定结果和除数同号的，也有不做明确规定的。

取模运算在程序中是非常有用的，例如上面的例子判断`x`的奇偶性（Parity），看`x`除以2的余数是不是0，如果是0则打印`x is even.`，如果不是0则打印`x is odd.`，读者应该能看出`else`在这里的作用了，如果在上面的例子中去掉`else`，则不管`x`是奇是偶，`printf("x is odd.\n");`总是执行。为了让这条语句更有用，可以把它封装（Encapsulate）成一个函数：

```
void print_parity(int x)
{
	if (x % 2 == 0)
		printf("x is even.\n");
	else
		printf("x is odd.\n");
}
```

把语句封装成函数的基本步骤是：_把语句放到函数体中，把变量改成函数的参数_。这样，以后要检查一个数的奇偶性只需调用这个函数而不必重复写这条语句了，例如：

```
print_parity(17);
print_parity(18);
```

`if/else`语句的语法规则如下：

语句 → if (控制表达式) 语句 else 语句

右边的“语句”既可以是一条语句，也可以是由{}括起来的语句块。一条`if`语句中包含一条子语句，一条`if/else`语句中包含两条子语句，子语句可以是任何语句或语句块，当然也可以是另外一条`if`或`if/else`语句。根据组合规则，`if`或`if/else`可以嵌套使用。例如可以这样：

```
if (x > 0)
	printf("x is positive.\n");
else if (x < 0)
	printf("x is negative.\n");
else
	printf("x is zero.\n");
```

也可以这样：

```
if (x > 0) {
	printf("x is positive.\n");
} else {
	if (x < 0)
		printf("x is negative.\n");
	else
		printf("x is zero.\n");
}
```

现在有一个问题，类似`if (A) if (B) C; else D;`形式的语句怎么理解呢？可以理解成

```
if (A)
	if (B)
		C;
else
	D;
```

也可以理解成

```
if (A)
	if (B)
		C;
	else
		D;
```

在[第 1 节 “继续Hello World”](https://akaedu.github.io/book/ch02s01.html#expr.helloworldcont)讲过，C代码的缩进只是为了程序员看起来方便，实际上对编译器不起任何作用，你的代码不管写成上面哪一种缩进格式，在编译器看起来都是一样的。那么编译器到底按哪种方式理解呢？也就是说，`else`到底是和`if (A)`配对还是和`if (B)`配对？很多编程语言的语法都有这个问题，称为Dangling-else问题。C语言规定，_`else`总是和它上面最近的一个`if`配对_，因此应该理解成`else`和`if (B)`配对，也就是按第二种方式理解。如果你写成上面第一种缩进的格式就很危险了：你看到的是这样，而编译器理解的却是那样。如果你希望编译器按第一种方式理解，应该明确加上{}：

```
if (A) {
	if (B)
		C;
} else
	D;
```

顺便提一下，浮点型的精度有限，不适合用==运算符做精确比较。以下代码可以说明问题：

```
double i = 20.0;
double j = i / 7.0;
if (j * 7.0 == i)
	printf("Equal.\n");
else
	printf("Unequal.\n");
```

不同平台的浮点数实现有很多不同之处，在我的平台上运行这段程序结果为`Unequal`，即使在你的平台上运行结果为`Equal`，你再把`i`改成其它值试试，总有些值会使得结果为`Unequal`。等学习了[第 4 节 “浮点数”](https://akaedu.github.io/book/ch14s04.html#number.float)你就知道为什么浮点型不能做精确比较了。

#### 习题 <a href="#id2718936" id="id2718936"></a>

1、写两个表达式，分别取整型变量`x`的个位和十位。

2、写一个函数，参数是整型变量`x`，功能是打印`x`的个位和十位。
