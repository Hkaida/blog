# JavaScript 函数对象

JavaScript 中的函数实际上就是对象，是 Object 的一个子类型，而不仅仅是能够“封装代码片段“的语法结构。

函数名就是变量，它保存的是对函数的引用。跟其他普通的数据类型一样，函数本体可以作为值赋给变量，也能以字面量形式声明，甚至能够作为函数参数传递或者从其他函数中返回。

--

## 一、函数是对象？

首先我们来看一下 JavaScript 对象的常见功能，对比函数到底有哪些相似之处。

1. 对象可以通过字面量创建，还可以赋值给变量、数组项，或者其他对象的属性：

```javascript
let arr = [];
let obj = {};

// 普通对象：
let person = {}; // 为变量赋值一个新对象
obj.person = person; // 赋值给其他对象的属性
arr.push(person); // 向数组添加一个对象

// 函数：
let sayHi = function () { // 为变量赋值一个新函数，这个函数是通过字面量创建的
    console.log('Hi');
}
obj.sayHi = sayHi; // 赋值给其他对象的属性
arr.push(sayHi); // 向数组添加一个函数
```

2. 对象可以作为函数参数传递：

```javascript
// 普通对象：
function fn(obj) {
    obj.a = 1; // 修改传入的对象
}
fn({ name: 'obj' });

// 函数：
function call(callback) {
    callback(); // 调用传入的函数
}
call(function() {
    console.log('我是 callback');
});
```

3. 对象可以作为函数的返回值：

```javascript
// 普通对象：
function fn() {
    return { name: 'obj' };
}
let obj = fn();
console.log(obj); // { name: 'obj' }

// 函数：
function fn2() {
    return function fn3() {
        console.log('我是 fn3');
    };
}
let foo = fn2(); // fn3 赋值给了 foo
foo(); // '我是 fn3'
```

4. 可以动态添加属性：

```javascript
let man = {};
man.name = '小明';
console.log(man); // { name: '小明' }

function fn() {
    console.log('我是 fn');
}
fn.author = '小明';
console.log(fn.author); // '小明'
```



从上面的例子可以看到，对象能做的，函数也能做。函数的特殊之处就在于它是可以调用的，即被调用后会执行某些动作。

--

### 函数作为对象的妙用

令人惊喜的是函数也可以动态添加属性，那么利用该特性来记忆一些跟函数自身相关的值，是一个不错的做法。

例如给函数添加一个 `count` 属性来跟踪调用的次数：

```javascript
function sayHi() {
	console.log("Hi");
    
	// 每次调用递增 1
	sayHi.count++;
}
sayHi.count = 0; // 初始化

sayHi();
sayHi();

console.log(`sayHi() 的调用次数：${sayHi.count}`);
```

> 属性不是变量
>
> `sayHi.count = 0;`  不会在函数内定义一个局部变量 `count ` 。属性 `count `  和 变量 `count ` 毫不相关。

--

## 二、函数的属性和方法

函数作为对象，它也有一些默认的属性和方法。

--

### 属性 name

可以通过属性 `name` 来获取函数名：

```javascript
function sayHi() {
	console.log("Hi");
}

console.log(sayHi.name); // sayHi
```

函数名的生成不仅限于函数声明，其他情况也能自动推测出名字：

```javascript
// 函数表达式：
let fn = function () {};
console.log(fn.name); // fn

// 对象方法：
let person = {
    sayHi() {
		console.log("Hi");
    }
}
console.log(person.sayHi.name); // sayHi

// 函数参数默认值：
function foo(sayHi = function() {}) {
	console.log(sayHi.name); // sayHi
}
foo();
```

但也有无法推测名字的情况，会导致属性 `name` 是一个空字符串：

```javascript
let arr = [function() {}];
console.log(arr[0].name);

// 匿名的立即调用函数
(function () {
    // arguments.callee 是函数本身
    console.log(arguments.callee.name);
})()
```

--

### 属性 length

可以通过属性 `length` 来获取函数的形参个数：

```javascript
function fn1() {}
function fn2(a) {}
function fn3(a, b) {}
function fn4(a, b, ...more) {}

console.log(fn1.length); // 0
console.log(fn2.length); // 1
console.log(fn3.length); // 2
console.log(fn4.length); // 2
```

需要注意的是，剩余参数（rest）不会被计算在内。

--

### 属性  caller 

该属性可以用来检测函数在何处被调用。

如果一个函数 `f` 在全局作用域中调用， 则 `f.caller`  的值为 `null` 。相反，如果函数 `f` 是在另外一个函数作用域内被调用的，则 `f.caller`  指向调用它的那个函数。

观察这个例子：

```javascript
function outer() { 
	inner(); 
} 
function inner() { 
	console.log(inner.caller); 
} 
outer();
```

以上会打印函数 `outer()` 的源代码，因为 `inner()` 是在 `outer()` 的作用域中被调用的。

--

### 属性 prototype

属性 prototype 保存着原型对象的引用，它是保存实例对象的共享方法的地方。例如 `toString()`、`valueOf()` 等方法都保存在原型对象上。

> 想要了解 prototype 的更多信息，请参见  → [F.prototype](https://zh.javascript.info/function-prototype)

--

### 方法 call()

`call()` 会以指定的 ` this` 值来调用函数/方法，即会设置运行时函数体内 `this` 的值。

**语法**

```javascript
function.call(thisArg, arg1, arg2, ...)
```

参数：

`thisArg` ：可选。为函数运行时的 `this` 指定一个对象。若传入 `null` 或缺省，那么 `this` 会替换为全局对象。

`arg1, arg2, ...`：函数本身需要的参数，可以在后面追加多个，使用逗号分隔。

观察这个例子：

```javascript
function add(num) {
    this.count += num;
}

let counter = {
    count: 0
}

// 此时 this 值为 counter，同时 1 传给了形参 num
// 等价于 counter.count += 1
add.call(counter, 1); 
console.log(counter.count); // 1
```

`call()` 让对象 `counter` 可以直接**借用**函数 `add()` ，来实现 `count` 属性进行计数。同样的，其他对象也可以通过这种方式来**借用**  `add()` ，通过这种形式能够实现不同对象共享一个函数/方法，而不是在新对象中再写一次相同功能的方法。

--

### 方法 apply()

`apply()` 和 `call()`  的作用一样，只是传参的方式不同。

**语法**

```javascript
function.call(thisArg, argArray)
```

参数：

`thisArg` ：同  `call()`  。

`argsArray`：函数本身需要的参数，它们需要放到一个数组或者类数组中。注意，这些参数传入给函数时，会自动展开为单独的参数，不会影响函数本身的参数接收形式。

观察这个例子：

```javascript
function add() {
    for (let i = 0; i < arguments.length; i++) {
        this.count += arguments[i];
    }
}

let counter = {
    count: 0
}

let arr = [1, 2];
add.apply(counter, arr); 
console.log(counter.count); // 3
```

`add()` 可以接收多个不固定数量的参数，并进行累加。如果需要将一个元素是数值的数组进行累加，使用`apply()` 明显会更方便。

但是用 `call()` 就一定会很麻烦吗？借助展开语法 `...` 即可。

```javascript
add.call(counter, ...arr); 
```

这两种方式几乎是等效的，但是 `apply()` 可能会更快，因为大多数 JavaScript 引擎在内部对其进行了优化。

所以到底是使用 `apply()` 还是 `call()`，完全取决于要如何给函数传参。如果想直接传 `arguments`对象或者一个数组，那就用 `apply()` ，否则，就用 `call()`。

--

### 方法 bind()

`bind()` 和 `call()` 的比较类似。不同之处在于 `bind()` 会创建一个新函数，这个新函数的 `this` 会永久绑定为 `bind()` 的第一个参数。 而其余参数也将永久绑定为新函数的参数，供调用时使用。 

观察这个例子：

```javascript
function add(num) {
    this.count += num;
}

let counter = {
    count: 0
}

let addCount = add.bind(counter, 1); // 不仅将 this 绑定了 counter，还绑定了参数
addCount(); // 每次执行都等价于 counter.count += 1
console.log(counter.count); // 1

addCount();
console.log(counter.count); // 2
```

新函数 `addCount()`  具备原函数 `add()` 的功能，区别就在于 `this` 总是等于 `counter`。如果不需要立即调用，或借用`add()` 不止一次的话， `bind()` 会比 `call()` 更方便。

上面的例子其实还存在一个问题，如果每次传入的参数都不是 1 呢？我们希望它更灵活一些，很简单，绑定的时候先不传入第二个及后面的参数，等到调用新函数时再传入即可：

```javascript
let addCount = add.bind(counter);
addCount(1);
addCount(2);
addCount(3);
```

另一个例子，面对多个参数的函数，有时候会这样：

```javascript
function mul(a, b) {
	return a * b;
}

let double = mul.bind(null, 2); // this 在这里不重要，可传入 null 作为占位符
console.log(double(1)); // 相当于 mul(2, 1)
console.log(double(2)); // 相当于 mul(2, 2)
console.log(double(3)); // 相当于 mul(2, 3)
```

这里基于一个乘法函数 `mul()`，创建了 `double()` 函数，进行部分参数绑定，实现将任何值都乘了两倍。不必每次都提供相同的参数 2 。

可以看到不管参数是先传还是后传，参数最终都能按顺序汇总，被“原样”传递。 这个特性在我们想要创建一个“不怎么灵活”的函数时非常有用。

-- 

## 总结

1. 函数也是对象，特殊之处在于它是可调用的。
2. 给函数添加属性不会影响局部变量。
3. `call()` 、`apply()` 、`bind()` 都可以设置运行时函数体内 `this` 的值。
   - `call()` 一次性设置 `this` 运行函数。
   - `apply()` 一次性设置 `this` 运行函数。可以自动展开数组传参给函数。
   - `bind()` 创建永久绑定 `this` 的新函数。同时可以绑定其余参数。

<center>- END -</center>