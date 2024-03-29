# JavaScript 数据类型


JavaScript 目前包含八种数据类型，其中可以分成两大类。

基本数据类型（原始值）：

- Number（数值，包含NaN）
- String（字符串）
- Boolean（布尔值）
- Undefined（未定义/未初始化）
- Null（空对象）
- Symbol（独一无二的值，ES6 新增）
- BigInt （大整数，能够表示超过 Number 类型大小限制的整数，ES 2020新增）

引用数据类型（引用值）：

- Object（对象。Array/数组 和 function/函数 也属于对象的一种）



基本数据类型的值是不可变的，你无法修改值本身，你只能给代表它的变量重新赋值，将原来的值覆盖。而引用数据类型是可变的——它们的值是可修改的。



## Number

Number 类型包括整数和浮点数值。也支持不同进制的表示方式。

基本的数值字面量格式是十进制整数，十进制整数可以像下面这样直接在代码中输入： 

```javascript
let intNum = 666; // 整数
```

八进制：

> 八进制的值第一位必须是零`0`，后面每一位数的范围在`0~7`。如果某一位数超出范围，首位的0会被忽略，后面的数值会按照十进制来解析

```javascript
let octalNum1 = 070; // 八进制的 56 
let octalNum2 = 079; // 无效的八进制数值——解析为 79 
let octalNum3 = 08; // 无效的八进制数值——解析为 8 
```

用前缀`0`表示的八进制字面量在严格模式下是无效的，会报错。需要用前缀`0o`表示。

十六进制：

> 十六进制的值前两位必须是`0x`，后面每一位十六进制数字的范围在`0~9及A~F`，字母A~F可以大写也可以小写。

```javascript
let hexNum1 = 0xA; // 十六进制的 10 
let hexNum2 = 0x1f; // 十六进制的 31
```

进行算术运算时，所有八进制和十六进制的数值都会被视为十进制。



#### 1. 浮点数值

浮点数的数值中必须包含一个小数点，并且小数点后面必须至少有一位数字。如果小数点后面没有数字，或者本身表示的就是有一个整数（例如1.0），那么该值会被转换为整数保存。

```javascript
let floatNum1 = 1.1; 
let floatNum2 = 1.; // 小数点后面没有数字——解析为 1 
let floatNum3 = .1; // 有效，但不推荐
let floatNum4 = 10.0; // 整数——解析为 10 
```

对于那些极大或极小的数值，可以用e表示法（即科学计数法）来表示。e后面的数字代表10的幂中的指数，该幂值用来与前面的数相乘。例子：

```javascript
let floatNum1 = 6.123e7; // 等于61230000
let floatNum2 = 3e-17; // 等于0.00000000000000003
```

浮点数值的最高精度是17位小数，但是进行算数运算时是有误差的。例如：

```javascript
let a = 0.1;
let b = 0.2;
console.log(a + b); // 结果是 0.30000000000000004
```

因此，应该要避免以下例子的测试：

```javascript
if (a + b == 0.3) { 
	// ...do something
}
```

要解决这个问题，可以将数字乘以10ⁿ转换为整数，计算最后再除以10ⁿ，但只能够减少误差，误差仍然会在某些计算中出现：

```javascript
console.log((0.1 * 10 + 0.2 * 10) / 10); // 0.3
console.log((0.28 * 100 + 0.14 * 100) / 100); // 0.4200000000000001 
```

最可靠的是使用 toFixed() 方法对计算结果进行四舍五入：

```javascript
console.log((0.1 + 0.2).toFixed(1)); // 0.3
console.log(((0.28 * 100 + 0.14 * 100) / 100).toFixed(2)); // 0.42
// 要注意的是 toFixed() 方法返回的是字符串，如果是将计算结果展示到页面上是很方便的。
// 如果是需要数值的情况，还要进行类型转换操作，例如使用"+"将其转换为数值：
console.log(+(0.1 + 0.2).toFixed(1)); // 0.3
console.log(typeof +(0.1 + 0.2).toFixed(1)) // number
```



#### 2. 数值范围

由于内存限制，JavaScript不能保存所有的数值。如果某次计算的结果得到了一个超出 JavaScript数值范围的值，那么这个数值将被自动转换成特殊的 Infinity 值。具体来说，如果这个数值是负数，则会被转换成-Infinity（负无穷），如果这个数值是正数，则会被转 换成 Infinity（正无穷）。 



#### 3. NaN

NaN，即非数值（Not a Number），这是一个特殊的数值，拥有以下两个特点：

1. 任何涉及 NaN 的操作（例如 NaN/10）都会返回 NaN。
2. NaN 与任何值都不相等，包括 NaN 本身。

鉴于NaN不等于自身，可以使用 isNaN() 全局函数来判断一个值是否是 NaN 值。该函数会对传入的参数尝试转换为数值（如果参数类型不是number的话），例如字符串“1”或者布尔值。任何不能被转换为数值的值都会导致该函数返回`true`。例如：

```javascript
console.log(isNaN(NaN));         // true 
console.log(isNaN(10));          // false（10是一个数值） 
console.log(isNaN("10"));        // false（可以被转换成数值 10） 
console.log(isNaN("blue"));      // true（不能转换成数值） 
console.log(isNaN(true));        // false（可以被转换成数值 1）
```



#### 4. 数值转换 

有 3个函数可以把其他类型的值转换为数值：

Number()：把任何数据类型的值转换为数值

parseInt()：把字符串转换为整数值

parseFloat()：把字符串转换为浮点数值



## String

字符串可以用双引号（ " ）、单引号（ ' ）或反引号（`）标识。以下三种形式都是合法的：

```javascript
let name1 = "小明";
let name2 = '小明';
let name3 = `小明`;
```



#### 1. 字符字面量

String 类型包含一些特殊的字符字面量，也叫转义序列，用于表示非打印字符，或者具有其 他用途的字符。这些字符字面量可以出现在字符串中的任意位置，而且也将被作为一个字符来解析。

| 字面量 | 含义                                                         |
| ------ | ------------------------------------------------------------ |
| \n     | 换行                                                         |
| \t     | 制表（跳到下一个TAB位置）                                    |
| \b     | 空格                                                         |
| \r     | 回车                                                         |
| \f     | 换页                                                         |
| \ \    | 反斜杠（ \ ）                                                |
| \ '    | 单引号（ ' ），在单引号表示的字符串中显示单引号              |
| \ "    | 双引号（ " ），在双引号表示的字符串中显示双引号              |
| \ \`    | 反引号（ \` ），在模板字符串中显示反引号                     |
| \Xnn   | 以十六进制代码nn表示的一个字符（其中n为0～F）。例如，\x41表示"A" |
| \unnnn | 以十六进制代码nnnn表示的一个Unicode字符（其中n为0～F）。例如，\u03a3表示希腊字符Σ |



#### 2. 转换为字符串

toString()：这是一个方法，数值、布尔值、对象和字符串值都有toString()方法。

String()：这是一个全局函数，适用于所有数据类型



#### 3. 模板字符串

模板字符串相当于加强版的字符串，可以定义多行字符串。还可以利用`${}`在字符串中插入变量和表达式。

```javascript
let s1 = `hello
world!`;
console.log(s1); 
// "hello"
// "world!"

let myName = '小明';
let s2 = `我叫${myName}`;
console.log(s2); 
// "我叫小明"

let s3 = `我今年${2020-2010}岁`;
console.log(s3); 
// "我今年10岁"
```



## Boolean

Boolean 类型只有两个字面值：`true` 和 `false`。这两个值与数字值不是一回事，因此 true 不一定等于 1，而 false 也不一定等于 0。



#### 1. 布尔值转换

其他 ECMAScript 类型的值都有与布尔值等价的形式。可以调用 Boolean() 函数来将其他类型转换为布尔值。不同类型转换为布尔值的规则如下表：

| 数据类型  | 转换为 true 的值       | 转换为 false 的值 |
| --------- | ---------------------- | ----------------- |
| Boolean   | true                   | false             |
| String    | 非空字符串             | ""（空字符串）    |
| Number    | 非零数值（包括无穷值） | 0、NaN            |
| Object    | 任意对象               | null              |
| Undefined | 无                     | undefined         |



## Undefined

Undefined 类型只有一个值，即`undefined`。在声明变量但是没有初始化时，这个变量的值就是undefined。

正常情况下，调用未声明的变量会报错，但是对未声明的变量使用typeof操作符，同样会返回"undefined"，例如：

```javascript
var myName;
// age 未声明
// var age;

console.log(typeof myName); // "undefined"
console.log(typeof age); // "undefined"
```



## Null

Null 类型也是只有一个值，即`null`。null表示一个空对象指针，因此使用 typeof 操作符检测 null 值时会返回"object"

```javascript
let car = null;
console.log( typeof car ); // "object"
```



## Symbol

Symbol 是 ES6 中引入的新基本类型，表示独一无二的值。Symbol 值通过 Symbol() 函数生成。

```javascript
let s = Symbol();
console.log(typeof s) // "symbol"
```

该函数可以接受一个字符串作为参数，表示对  Symbol 实例的描述，使得在控制台显示，或者转换为字符串进行判断时容易区分。

```javascript
let s1 = Symbol('foo');
let s2 = Symbol('bar');

console.log(s1) // "Symbol(foo)"
console.log(s2) // "Symbol(bar)"

if ( s1.toString() === "Symbol(foo)" ) {
    // do something...
}
```

> 注意， Symbol() 函数的参数只有描述作用，即使两个 Symbol() 函数的参数相同 ，它们的返回值也是不相等的。



#### 1. 实例：Symbol 值作为对象的属性名

因为每一个 Symbol 值都是不相等的，所以 Symbol 值可以用作唯一标识，用于对象的属性名，就能防止属性名的同名冲突。对于一个对象是由多个模块构成的情况下非常有用，避免某个 key 被改写或者覆盖。

```javascript
let s = Symbol();

// 写法一
let a = {};
a[s] = 'Hello!';

// 写法二
let a = {
  [s]: 'Hello!'
};

console.log(a[s]); // "Hello!"
```

注意，Symbol 值作为对象属性名时，不能用点运算符，需要用中括号运算符。否则对象`a`的属性名`s`会被认为是一个字符串而不是一个 Symbol 值。

同理，在对象的内部使用 Symbol 值定义属性时，Symbol 值也必须放在方括号之中（参考以上代码写法二）。



#### 2. 方法 Symbol.for(key)

该方法和`Symbol()`都能生成新的 Symbol，区别是前者会将这个Symbol 注册到**全局 symbol 注册表**，而后者不会。

`Symbol.for()` 调用不会直接生成新的 Symbol ，它会先从注册表中搜索给定的`key`是否存在。如果存在，那么返回的是同一个 Symbol，否则生成新的。

```javascript
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true
```



#### 3. 方法 Symbol.keyFor(symbol)

该方法返回一个已注册的 Symbol 类型值的key。

```javascript
// 创建一个 symbol 并注册，key 为 "foo"
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

// 创建一个 symbol 但不注册，注册表中找不到该symbol 的 key
let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```



## BigInt

BigInt 是 ES 2020 新增的基本类型，能够表示任意长度的整数，突破了 Number 类型的大小限制问题。

要定义一个 BigInt ，可以在整数字面量加上`n`的后缀，或者调用`BigInt()`函数。

```javascript
let b1 = 10n;
let b2 = 9007199254740991n;
let b3 = BigInt(Number.MAX_SAFE_INTEGER); 
let b4 = BigInt(Number.MIN_SAFE_INTEGER);
let b5 = BigInt("0x1fffffffffffff");

console.log(b1); // 10n
console.log(b2); // 9007199254740991n
console.log(b3); // 9007199254740991n
console.log(b4); // -9007199254740991n
console.log(b5); // 9007199254740991n
```

上面例子中的 Number.MAX_SAFE_INTEGER 和 Number.MIN_SAFE_INTEGER 表示最大和最小安全整数，如果是Number 类型数值超过此范围都可能会失去精度。

```javascript
console.log(9999999999999999) // 10000000000000000
```



#### 1. 运算

BigInt 支持除单目`+`运算符以外的算数运算符。

```javascript
console.log(10n + 10n); // 20n
console.log(11n - 10n); // 1n
console.log(2n * 10n); // 20n
console.log(110n / 10n); // 11n
console.log(10n ** 3n); // 1000n
console.log(101n % 10n); // 1n

console.log(-10n); // -10n
console.log(+10n); // 报错
```

BigInt 运算结果如果有小数则会被向下取整。

```javascript
console.log(5n / 2n); // 2n, 而非2.5n
```



#### 2. 注意点

BigInt 虽然在一些方面类似 Number，但也有一些需要注意的地方：

1. 不能用 Math 对象中的方法。
2. 不能和 Number 类型数值进行混合算术运算，必须转换成同一类型。
3. 虽然 BigInt 不能和 Number 直接进行算术运算，但使用比较运算符（`==`、`<`、`>`等等）直接比较是允许的。
4. BigInt 转换为 Number 可能会丢失精度。 



## Object

对象是一组数据和方法的无序集合，所有实例的基础是Object 类型。



#### 1. 创建对象

可以通过创建 Object 实例来创建自己的对象，方式有两种，第一种是使用 new 操作符和 Object 构造函数：

```javascript
let person = new Object(); // 创建了一个空对象
person.name = "小明"; // 往对象中添加属性
person.age = 20;
```

第二种方式是使用**对象字面量**表示法，能够简化对包含大量属性的对象的创建，看起来也更加一目了然：

```javascript
let person = {
	name: "小明",
	age: 20
};
```

以上两种方式是等效的，但是要注意，在使用对象字面量的方式定义对象时，并不会实际调用 Object 构造函数。另外，对象字面量法中，对象属性名也可以用字符串或者数值，若是数值属性则会自动转换为字符串。

```javascript
let person = {
	"name": "小明",
	"age": 20,
    5: true
};
```



#### 2. 对象属性存取

对象的属性一般是通过**点语法**来进行存取，但是也可以用中括号来存取属性。使用中括号时，要在中括号内使用属性名的字符串形式，例如：

```javascript
console.log(person.name); // "小明"
console.log(person["name"]); // "小明"
```

通常，点语法是首选的存取方式，必须使用中括号语法的情况有：

1. 需要通过变量来访问属性。
2. 属性名中包含可能会导致语法错误的字符，例如关键字/保留字、空格等。
3. 属性名是数值

```javascript
let propertyName = "name";
console.log(person[propertyName]); // "小明"

person["my name"] = "小明";

person[5] = false;
```



#### 3. 拓展

对象大致可分为三类：

- 内置对象：由 ECMAScript 定义的对象，例如Array、Math、Date。
- 自定义对象：通过构造函数或者字面量创建的对象。
- 宿主对象：由执行环境提供的对象，例如浏览器环境的DOM和BOM中的对象



Object 的每个实例都具有以下属性和方法：

- constructor：指向构造函数。 
- hasOwnProperty(propertyName)：检查某个属性是否在当前对象实例中（而不是在实例的原型中）。参数必须为字符串形式。
- isPrototypeOf(object)：用于检查调用对象是否是传入对象的原型。
- propertyIsEnumerable(propertyName)：用于检查某个属性是否能够使用 for-in 语句来枚举。参数必须为字符串形式。
- toLocaleString()：返回对象的字符串表示，该字符串与执行环境的地区对应。JavaScript 的一些内置对象都重写了该函数，以实现更适合自身的功能需要。
- toString()：返回对象的字符串表示。
- valueOf()：返回对象的字符串、数值或布尔值表示。通常与 toString()方法的返回值相同。

> ECMAScript 中对象的行为不一定适用于 JavaScript 中的其他对象。 例如宿主对象不是由 ECMAscript 定义的，因此宿主对象不一定会继承 Object。



<center>- END -</center>

