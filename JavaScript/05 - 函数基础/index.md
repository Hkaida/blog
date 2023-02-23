# JavaScript 函数基础

函数的主要作用就是把程序中许多相似的操作封装起来，达到复用的目的，需要时调用一下函数即可，不用再编写过多的重复代码，大大简化了后续相似的操作。

--

## 一、函数声明

函数使用 function 关键字来声明，基本语法如下：

```javascript
function 函数名(参数1, 参数2, ...) {
    // 代码编写在这里
    
    return value;
}
```

可以通过函数名加上`()`来调用函数，就像这样：

```javascript
function sayHi() {
	console.log('Hello!');
}

sayHi(); // 输出 'Hello!' 
sayHi(); // 可复用
```

--

## 二、作用域

函数会形成一个作用域，换句话说就是，函数中声明的变量只在该函数内部可见。

```javascript
function fn() {
    let a = 1;
    console.log(a); 
}

fn();// 输出 1
console.log(a); // 报错，外部无法访问函数内部的变量
```

外部无法访问函数内部的变量，但是函数可以访问外部的变量，也可以对其修改。

```javascript
let a = 1;

function fn() {
    a += 1;
    console.log(a); 
}

fn();// 输出 2
console.log(a); // 2 ，外部的 a 已经被修改
```

如果函数内部声明了跟外部同名的变量，它们不是在同一个作用域中的，是不同的两个变量。函数中访问同名变量时，会使用函数内部变量，忽略外部变量。

> 在函数之外声明的变量（也不在{}之内）被称为全局变量，它属于全局作用域。全局变量在所有函数中都是可见的，但是也有弊端——它们有被意外修改的风险，尽量减少全局变量的使用是一种很好的做法。
>
> 想要了解更多关于作用域的内容，请参见 → [作用域](https://blog.csdn.net/weixin_48276895/article/details/109325171)

--

## 三、参数

我们可以通过参数将数据传递给函数，通过传递的数据影响执行结果，使函数更灵活、复用性更强。

```javascript
function foo(a, b) {
    console.log([a, b]);
}

foo(1, 2); // 输出 [1, 2]
```

这个例子中，`a` 和 `b` 属于函数中的局部变量，只能在函数中访问。调用函数时，传递的数据会根据位置来匹配对应，分别赋值给 `a` 和 `b`。

创建函数时，`function 函数名` 后面括号中设定的参数被称为**形参**；调用函数时，函数名后面括号中传入的参数被称为**实参**。上面例子中，`a` 和 `b` 是形参，传入的 `1` 和 `2` 是实参。

--

#### 参数默认值

如果调用函数时缺少提供实参，那么形参默认值为 `undefined`。

有时候我们想要设置默认值，在 ES6 之前还不支持显式地设置默认值，只能采用变通的方式：

```javascript
function sayHi(name) {
    name = name || 'everyone';
	console.log( 'Hello ' + name + '!');
}

sayHi(); // 输出 'Hello everyone!' 
```

通过检查参数值的方式判断有没有赋值，上面的做法虽然简便，但缺点在于如果传入的参数对应布尔值为 `false` ，实参就不起作用了。需要更精确的话可以用 `if` 语句或者三元表达式，判断参数是否等于 `undefined`，如果是则说明这个参数缺失 ：

```javascript
// if 语句判断
function sayHi(name) {
	if (name === undefined) {
		name = 'everyone';
	}
    
	console.log( 'Hello ' + name + '!');
}

// 三元表达式判断
function sayHi(name) {
	name =  (name !== undefined) ? name : 'everyone';
	
    console.log( 'Hello ' + name + '!');
}
```

ES6 就方便了许多，因为它支持了显式的设置默认值的方式，就像这样：

```javascript
function sayHi(name = 'everyone') { // 定义函数时，直接给形参赋值
	console.log( 'Hello ' + name + '!');
}

sayHi(); // 输出 'Hello everyone!' 
sayHi('Tony'); // 输出 'Hello Tony!' 
sayHi(undefined); // 输出 'Hello everyone!'
```

这些结果表明了，它也是通过参数是否等于 `undefined` 来判定参数是否缺失的。

默认值不但可以是一个值，它还可以是任意合法的表达式，甚至是函数调用：

```javascript
function sayHi(name = 'every'+'one') {
	console.log( 'Hello ' + name + '!');
}

function sayHi(name = foo()) {
	console.log( 'Hello ' + name + '!');
}
```

--

>关于函数参数还有一些其他的细节和技巧，具体可参见 → [函数参数](https://blog.csdn.net/weixin_48276895/article/details/110501121)

--

## 四、返回值

函数中可以使用 `return` 关键字将一个值作为调用的结果返回。例如：

```javascript
function sum(a, b) {
    return a + b;
}

let result = sum(1, 2); // 用变量来接收返回的值

console.log(result); // 3
```

`return` 关键字是可选的，它有两个作用：

1. 指定返回值。如果不使用 return ，或者用 return 而不指定返回的值，则默认返回 `undefined` 。
2. 结束函数的执行。当函数执行到 return 语句，函数会立即停止执行并退出，return 语句后面哪怕还有代码也不会继续执行。

```javascript
function fn1(){
    
}
function fn2(){
    return
}

console.log( fn1() === undefined ); // ture
console.log( fn2() === undefined ); // ture
```

`return `可以在函数中存在多个，例如：

```javascript
function diff(num1, num2) {
    if (num1 < num2) {
        return num2 - num1; // 若条件符合则执行此条语句，
    }
    
    return num1 - num2;
}

diff(1,2); // 1
diff(3,1); // 2
```

上面例子中，当 `num1 < num2` 成立，则执行完第一条 return 语句并终止函数执行，否则执行第二条 return 语句。

**注意**！不要在 return 和返回值之间换行。对于返回一条比较长的表达式，你可能会出于可读性的目的把它放在新的一行，但是：

```javascript
return // 等于 return;
	(some + long + expression + or + whatever * f(a) + f(b))
```

这样不行，因为 JavaScript 默认会在 return 语句最后加上分号`;`，实际上这样会返回 `undefined`。如果实在想跨行，那么应该在 `return` 这一行开始写，或者按照如下方式加上括号：

```javascript
return (
	some + long + expression
	+ or +
	whatever * f(a) + f(b)
	)
```

> 推荐的做法是要么让函数始终都返回一个值，要么永远都不要返回值。否则，如 果函数有时候返回值，有时候有不返回值，会给调试代码带来不便。 

--

## 五、其他形式的函数

JavaScript 中创建函数的方式不止一种。

--

#### 函数表达式

在 JavaScript 中，函数不是“神奇的语言结构”，而是一种特殊的值，就像 10 或者 [1, 2, 3] 这些一样。因此，有另一种创建函数的方式被称为**函数表达式**。就像这样：

```javascript
let sayHi = function() {
	console.log('Hello!');
}
```

可以看到这种创建函数的方式和其他赋值一样，把一个**匿名函数**赋值给一个变量。不管函数是采用何种定义方式，都只是一个存储在变量 `sayHi` 的值。

> 匿名函数即 function 关键字后面没有标识符。如果一个函数有其他方式能够被调用，那么 function 关键字后面可以省略标识符。但如果是函数声明来创建函数，则不能省略。

函数表达式和函数声明除了创建方式不同之外，还有两个不同之处。

1. **在预解析时它们的“提升”行为不同。**

   函数声明：会提升整个函数体，所以能在定义之前调用。

   函数表达式：跟其他变量一样，只提升标识符，不提升其初始化。

>想要了解更多关于预解析时 “提升” 的内容，请参见 → [变量提升和函数提升](https://blog.csdn.net/weixin_48276895/article/details/109409290)

2. **严格模式下，函数声明如果在`{}`中，它的声明会被限制在块级作用域内。**

以下代码期望的是根据条件声明函数，但是并不能达到目的：

```javascript
'use strict';
let a = 1;
if (a) {
	foo(); // 1
	function foo() {
		console.log(1);
	}
} else {
	function foo() {
		console.log(0);
	}
}

foo(); // 报错，foo 没有定义
```

它的作用域已经被限制在块级作用域内，只在`{}`内可见。那怎么样才能实现条件声明呢，可以使用函数表达式：

```javascript
'use strict';
let a = 1;
let foo;
if (a) {
	foo = function () {
		console.log(1);
	}
} else {
	foo = function () {
		console.log(0);
	}
}

foo(); // 输出 1
```

-----

##### 命名函数表达式

刚才所提到的函数表达式，它是匿名的。相对的，也可以在 function 关键字后面加上名字，这种被称为**命名函数表达式（NFE，Named Function Expression）**。

它看起来是这样：

```javascript
let sayHi = function fn() {
	console.log('Hello!');
}

sayHi(); // 输出 'Hello!'
```

它依然是函数表达式，那么多了这一个名字的意义是什么呢？

这里 `fn` 有两个特殊的地方：

1. 它允许函数在内部引用自己。
2. 在函数外部不可见。

```javascript
let sayHi = function fn( name ) {
	if (name) {
		console.log('Hello' + name + !);
	} else {
		fn('everyone!'); // 调用自身
	}
};

sayHi(); // Hello, everyone

func(); // 报错, 在函数外部不可见
```

为什么不用 `sayHi` 来引用自己，而要用 `fn` 来引用自己呢？`sayHi` 当然在大多数情况下都可以引用自己，但如果是下面这种情况：

```javascript
let sayHi = function( name ) {
	if (name) {
		console.log('Hello' + name + !);
	} else {
		sayHi('everyone!');
	}
};
let welcome = sayHi;
sayHi = null;

welcome(); // 报错，sayHi 不再是一个函数
```

 `sayHi` 是有可能被函数外部代码改变的，当它不再指向这个函数，那么函数就会报错。但如果提供了一个内部的函数名字 ，就可以避免这个问题。另外，在函数内部也不可改变它，这里的 `fn` 是一个常量。

```javascript
let sayHi = function ( name ) {
 	sayHi = 1;
 	console.log(sayHi);
 };

sayHi(); // 输出 1
sayHi(); // 报错，sayHi 不再是函数


let sayHi = function fn( name ) {
 	fn = 1;
 	console.log(fn);
 };

sayHi(); // 报错，Assignment to constant variable. 常量不可改变
```

--

##### 立即调用函数表达式

前面提到的函数表达式，它们需要通过函数名去调用才会执行，还有另一种方式可以执行函数表达式，这种方式通常被称为**立即调用函数表达式（IIFE，immediately-invoked function expressions）**。

通常是这样的：

```javascript
(function () {
    console.log('IIFE');
})(); // 最后的括号代表调用

(function () {
    console.log('IIFE');
}());  // 括号第二种放置方式，功能是一样的
```

函数首先被包裹在一对括号内，使其成为一个表达式，所以不需要函数名，而第二个括号代表调用这个函数。

除了使用括号，还有其他方式可以使函数形成一个表达式：

```javascript
!function() {
  alert('利用 ! 运算符形成表达式');
}();

+function() {
  alert('利用 + 运算符形成表达式');
}();
```

立即调用函数表达式的主要作用是用来创建一个局部的作用域，当然它也可以进行参数传递。

```javascript
let a = 1;

(function (value) {
    console.log(value);
})(a);
```

立即调用函数表达式的主要作用是用来形成一个局部的作用域，作为模仿块级作用域的存在，在 ES6 支持块级作用域之后，已经不再需要创建立即调用函数表达式这样的方式。 但我们依然要知道它，能够帮助我们去理解可能会遇到的旧标准代码。

--

#### 箭头函数

ES6 提供了新的创建函数方式，这种方式类似于函数表达式但更为方便。它就是箭头函数，基本语法如下：

```javascript
let sum = (a, b) => {
    return a + b;
}
```

在只有一个参数的情况下，可以省略括号

```javascript
let sum = x => { return ++x; }
```

如果只有一行代码，甚至可以省略大括号`{}` 和 `return` ，它会隐式地返回这行代码的结果。

```javascript
let sum = x => ++x;
```

--

> 箭头函数还有其他有趣的特性，具体可参见 → [箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

--

#### new Function 语法

最后一种创建函数的方式是利用构造函数 `Function`。

```javascript
let sum = new Function('a', 'b', 'return a + b');

console.log(sum(1, 2)); // 3
```

`Function` 构造函数接收字符串参数，将字符串转换成函数。最后一个参数为**函数体**，而之前的参数都是**函数参数**。

下面是一个没有参数的例子：

```javascript
let sayHi = new Function('console.log("Hello")');

sayHi(); // 输出 'Hello'
```

这样的函数在内部不能访问外层作用域（非全局）的变量，只能直接访问全局变量（不是传递参数而是直接引用）。

```javascript
let value = "global";

function getValue() {
	let value = "test";

	let fn = new Function('console.log(value)');
	
    return fn;
}

getValue()(); // 输出 'global' ; 如果没有全局变量 value，会报错：value 未定义
```

这种定义函数的方式并不推荐使用，因为它会被解释两次：第一次是将它当做常规的代码，第二次是解释传给构造函数的字符串。明显影响了性能。

--

## 六、何时使用函数

函数复用性的特点十分明显，通常会认为用函数只是封装多次使用的代码。但把一段逻辑相关的代码封装在一起也很有用，即使只调用一次。例如下面一个获取随机整数的函数：

```javascript
function getRandomInt(min, max) {
	min = Math.ceil(min);
	max = Math.floor(max);
	return Math.floor(Math.random() * (max - min)) + min; // 不含最大值，含最小值
}

let result = getRandomInt(1, 10);
```

在理解代码逻辑时，只需要把关注点放在 `let result = getRandomInt(1, 10);` 语句上，代码结构更为清晰，可读性更强。

--

## 总结

1. 函数的创建有 4 种方式：函数声明、函数表达式、箭头函数、new Function 语法。
2. 函数声明和函数表达式区别在于 “提升” 和在`{ }`中的表现。函数声明会提升整个函数体，严格模式下会被限制在块级作用域中；而函数表达式的提升和可见范围取决于变量的声明方式。
3. 如果函数要在内部要引用自己，推荐使用函数声明创建函数，或者使用命名函数表达式。
4. 立即调用函数表达式主要是用来模拟块级作用域，编写新的代码不需要用到它，学习它主要是为了帮助理解旧标准的代码。
5. 有时候即使一段逻辑相关的代码只会出现一次，把它们封装成一个函数也很有意义。

<p style="text-align: center">- END -</p>