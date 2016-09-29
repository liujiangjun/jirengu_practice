# 问答

## 1\. CSS和JS在网页中的放置顺序是怎样的？

CSS最好放在`<head>`标签里,有以下注意

> 注意:使用`link`标签将样式表放在顶部

如果放到底部,会出现什么问题

**白屏问题**

如果把样式放在底部,对于IE浏览器,在某些场景下(新窗口打开,刷新等)页面会出现白屏,而不是内容逐步展现

如果使用`@import`标签,即使CSS放入`link`, 并且放在头部,也可能出现白屏

**FOUC (Flash of Unstyled Content) 无样式内容闪烁**

如果把样式放在底部,对于IE浏览器,在某些场景下(点击链接,输入URL,使用书签进入等),会出现FOUC现象(逐步加载无样式的内容,等CSS加载后页面突然展现样式).对于Firefox会一直表现出FOUC.

JS呢

> 注意:将JS放在底部

如果放到顶部,会出现什么问题

- 脚本会阻塞后面内容的呈现
- 脚本会阻塞其后组件的下载

对于图片和CSS, 在加载时会并发加载(如一个域名下同时加载两个文件). 但在加载 JavaScript 时,会禁用并发,并且阻止其他内容的下载. 所以把 JavaScript 放入页面顶部也会导致白屏现象.

## 2\. 解释白屏和FOUC

什么是白屏

当浏览器获取到内容但没有得到CSS样式时，对于无样式内容不会进行渲染，所以这时候就会出现白屏现象。

在IE浏览器当中，如果把样式放在底部，在一些场景当中，比如刷新页面、新窗口打开等，页面就会出现白屏，而不是内容逐步展现。

如果使用@import标签，即使CSS放入link当中且也放在头部，也有可能会出现白屏现象。

对于图片和CSS，在加载时会并发加载。然而在加载JS文件时会禁用并发，阻止其他内容的下载。所以把JS代码放在页面顶部也会造成白屏现象。

什么是FOUC

FOUC (Flash of Unstyled Content) ，即无样式内容闪烁。它指的是这样一种现象：逐步加载无样式的内容，等CSS加载完成后页面突然展现的样子。也就是说，浏览器加载了无样式内容，又突然解析到了样式，会对页面进行重新的渲染，这时候就会产生FOUC现象。

在IE浏览器中，如果把样式放在底部，某些场景下（点击链接、输入URL、使用书签进入等等），就会出现FOUC现象。对于 Firefox 则会一直表现出 FOUC。

## 3\. async和defer的作用是什么？有什么区别。

这涉及到加载异步的概念

比如一下代码

```
<script src="script.js"></script>
```

没有`defer`或`async`，浏览器会立即加载并执行指定的脚本，"立即"指的是在渲染该`script`标签之下的文档元素之前，也就是说不等待后续载入的文档元素，读到就加载并执行。

- `<script async src="script.js"></script>`有`async`，加载和渲染后续文档元素的过程将和`script.js`的加载与执行并行进行（异步）。
- `<script defer src="script.js"></script>` 有`defer`加载后续文档元素的过程将和`script.js`的加载并行进行（异步），但`script.js`的执行要在所有元素解析完成之后，DOMContentLoaded 事件触发之前完成。
- `async`:不保证顺序
- `defer`:脚本延迟到文档解析和显示后执行，有顺序(用的会比较多)

## 4\. 简述网页的渲染机制

- 解析HTML标签, 构建DOM树
- 解析CSS标签, 构建CSSOM树
- 把DOM和CSSOM组合成渲染树(render tree)
- 在渲染树的基础上进行布局, 计算每个节点的几何结构
- 把每个节点绘制到屏幕上 (painting)

## 5\. JavaScript 定义了几种数据类型? 哪些是简单类型?哪些是复杂类型?

> 一句话:数据类型氛围四大类:原始类型,特殊值`null`,特殊值`undefined`和对象,其中ing原始类型包括数值,字符串和布尔值,对象包括狭义的对象,数组和函数.

JavaScript语言的每一个值，都属于某一种数据类型。JavaScript的数据类型，共有六种,分别为数值(number),字符串(string),布尔值(boolean),特殊值`null`,特殊值`undefined`和对象(object)

通常，我们将数值、字符串、布尔值称为原始类型（primitive type）的值，即它们是最基本的数据类型，不能再细分了。而将对象称为合成类型（complex type）的值，因为一个对象往往是多个原始类型的值的合成，可以看作是一个存放各种值的容器。至于`undefined`和`null`，一般将它们看成两个特殊值。

- 原始类型

  - 数值(number)
  - 字符串(string)
  - 布尔值(boolean)

- 特殊值:`null`(比较特殊,`typeof`后返回`object`)

- 特殊值:`undefined`

- 对象(object)

  - 狭义的对象(object)
  - 数组(array)
  - 函数(function)

狭义的对象和数组是两种不同的数据组合方式，而函数其实是处理数据的方法。JavaScript把函数当成一种数据类型，可以像其他类型的数据一样，进行赋值和传递，这为编程带来了很大的灵活性，体现了JavaScript作为"函数式语言"的本质。

这里需要明确的是，JavaScript的所有数据，都可以视为广义的对象。不仅数组和函数属于对象，就连原始类型的数据（数值、字符串、布尔值）也可以用对象方式调用。

## 6\. NaN、undefined、null分别代表什么?

### NaN

> 一句话:`NaN`的数据类型属于`number`,表示非数字,不等于任何值(包括自己本身),与任何数值运算都得到`NaN`,可用`isNaN`函数判断是否是`NaN`

（1）含义

`NaN`是JavaScript的特殊值，表示"非数字"（Not a Number），主要出现在将字符串解析成数字出错的场合。

```
5 - 'x' // NaN
```

上面代码运行时，会自动将字符串`x`转为数值，但是由于`x`不是数值，所以最后得到结果为`NaN`，表示它是"非数字"（`NaN`）。

另外，一些数学函数的运算结果会出现`NaN`。

```
Math.acos(2) // NaN
Math.log(-1) // NaN
Math.sqrt(-1) // NaN
```

`0`除以`0`也会得到`NaN`。

```
0 / 0 // NaN
```

需要注意的是，`NaN`不是一种独立的数据类型，而是一种特殊数值，它的数据类型依然属于`Number`，使用`typeof`运算符可以看得很清楚。

```
typeof NaN // 'number'
```

（2）运算规则

`NaN`不等于任何值，包括它本身。

```
NaN === NaN // false
```

由于数组的`indexOf`方法，内部使用的是严格相等运算符，所以该方法对`NaN`不成立。

```
[NaN].indexOf(NaN) // -1
```

`NaN`在布尔运算时被当作`false`。

```
Boolean(NaN) // false
```

`NaN`与任何数（包括它自己）的运算，得到的都是`NaN`。

```
NaN + 32 // NaN
NaN - 32 // NaN
NaN * 32 // NaN
NaN / 32 // NaN
```

（3）判断NaN的方法

`isNaN`方法可以用来判断一个值是否为`NaN`。

```
isNaN(NaN) // true
isNaN(123) // false
```

但是，`isNaN`只对数值有效，如果传入其他值，会被先转成数值。比如，传入字符串的时候，字符串会被先转成`NaN`，所以最后返回`true`，这一点要特别引起注意。也就是说，`isNaN`为`true`的值，有可能不是`NaN`，而是一个字符串。

```
isNaN('Hello') // true
// 相当于
isNaN(Number('Hello')) // true
```

出于同样的原因，对于对象和数组，`isNaN`也返回`true`。

```
isNaN({}) // true
// 等同于
isNaN(Number({})) // true

isNaN(['xzy']) // true
// 等同于
isNaN(Number(['xzy'])) // true
```

但是，对于空数组和只有一个数值成员的数组，`isNaN`返回`false`。

```
isNaN([]) // false
isNaN([123]) // false
isNaN(['123']) // false
```

上面代码之所以返回`false`，原因是这些数组能被`Number`函数转成数值

因此，使用isNaN之前，最好判断一下数据类型。

```
function myIsNaN(value) {
  return typeof value === 'number' && isNaN(value);
}
```

判断`NaN`更可靠的方法是，利用`NaN`是JavaScript之中唯一不等于自身的值这个特点，进行判断。

```
function myIsNaN(value) {
  return value !== value;
}
```

### undefined和null

> 一句话:两者差不多,`Boolean`都返回`false`,`==`甚至认为二者相等,不过区别还是有的,不要太在意

`null`与`undefined`都可以表示"没有"，含义非常相似。将一个变量赋值为`undefined`或`null`，老实说，语法效果几乎没区别。

```
var a = undefined;
// 或者
var a = null;
```

上面代码中，`a`变量分别被赋值为`undefined`和`null`，这两种写法的效果几乎等价。

在`if`语句中，它们都会被自动转为`false`，相等运算符（`==`）甚至直接报告两者相等。

```
if (!undefined) {
  console.log('undefined is false');
}
// undefined is false

if (!null) {
  console.log('null is false');
}
// null is false

undefined == null
// true
```

上面代码说明，两者的行为是何等相似！Google公司开发的JavaScript语言的替代品Dart语言，就明确规定只有`null`，没有`undefined`！

既然含义与用法都差不多，为什么要同时设置两个这样的值，这不是无端增加复杂度，令初学者困扰吗？这与历史原因有关。

1995年JavaScript诞生时，最初像Java一样，只设置了`null`作为表示"无"的值。根据C语言的传统，`null`被设计成可以自动转为`0`。

```
Number(null) // 0
5 + null // 5
```

但是，JavaScript的设计者Brendan Eich，觉得这样做还不够，有两个原因。首先，`null`像在Java里一样，被当成一个对象。但是，JavaScript的值分成原始类型和合成类型两大类，Brendan Eich觉得表示"无"的值最好不是对象。其次，JavaScript的最初版本没有包括错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。Brendan Eich觉得，如果`null`自动转为`0`，很不容易发现错误。

因此，Brendan Eich又设计了一个`undefined`。他是这样区分的：`null`是一个表示"无"的对象，转为数值时为`0`；`undefined`是一个表示"无"的原始值，转为数值时为 `NaN`。

```
Number(undefined) // NaN
5 + undefined // NaN
```

但是，这样的区分在实践中很快就被证明不可行。目前`null`和`undefined`基本是同义的，只有一些细微的差别。

`null`的特殊之处在于，JavaScript把它包含在对象类型（object）之中。

```
typeof null // "object"
```

上面代码表示，查询`null`的类型，JavaScript返回`object`（对象）。

这并不是说`null`的数据类型就是对象，而是JavaScript早期部署中的一个约定俗成，其实不完全正确，后来再想改已经太晚了，会破坏现存代码，所以一直保留至今。

下面是返回`undefined`的场景

```
// 变量声明了，但没有赋值
var i;
i // undefined

// 调用函数时，应该提供的参数没有提供，该参数等于undefined
function f(x) {
  return x;
}
f() // undefined

// 对象没有赋值的属性
var  o = new Object();
o.p // undefined

// 函数没有返回值时，默认返回undefined
function f() {}
f() // undefined
```

## typeof和instanceof的作用和区别?

![typeof运算符](http://ocpxqfoch.bkt.clouddn.com/6bb63c5067be0f0f9e965a2c898ecb10.png)

`typeof`是用来确定一个值到底属于什么数据类型

JavaScript有三种方法，可以确定一个值到底是什么类型。

- `typeof`运算符
- `instanceof`运算符
- `Object.prototype.toString`方法

`typeof`运算符可以返回一个值的数据类型，可能有以下结果。

数值、字符串、布尔值分别返回`number`、`string`、`boolean`。

```
typeof 123 // "number"
typeof '123' // "string"
typeof false // "boolean"
```

函数返回 `function`。

```
function f() {}
typeof f
// "function"
```

`undefined`返回`undefined`。

```
typeof undefined
// "undefined"
```

除此以外，其他情况都返回`object`。

```
typeof window // "object"
typeof {} // "object"
typeof [] // "object"
typeof null // "object"
```

既然`typeof`对数组（array）和对象（object）的显示结果都是`object`，那么怎么区分它们呢？`instanceof`运算符可以做到。

```
var o = {};
var a = [];

o instanceof Array // false
a instanceof Array // true
```

# 代码

## 1\. 完成如下代码判断一个变量是否是数字、字符串、布尔、函数

ps: 做完后可参考[underscore.js](http://www.bootcss.com/p/underscore/docs/underscore.html)源码中部分实现

```
function isNumber(el){
    // todo ...
}
function isString(el){
    //todo ...
}
function isBoolean(el){
    //todo ...
}
function isFunction(el){
    //todo ...
}

var a = 2,
    b = "jirengu",
    c = false;
alert( isNumber(a) );  //true
alert( isString(a) );  //false
alert( isString(b) );  //true
alert( isBoolean(c) ); //true
alert( isFunction(a)); //false
alert( isFunction( isNumber ) ); //true
```

作答如下

```
function isNumber(e1){
  return typeof e1 === 'number';
}
function isString(e1){
  return typeof e1 === 'string';
}
function isBoolean(e1){
  return typeof e1 === 'boolean';
}
function isFunction(e1){
  return typeof e1 === 'function';
}
var a = 2,
b = "jirengu",
c = false;
alert( isNumber(a) ); //true
alert( isString(a) ); //false
alert( isString(b) ); //true
alert( isBoolean(c) ); //true
alert( isFunction(a)); //false
alert( isFunction( isNumber ) ); //true
```

## 2\. 以下代码的输出结果是?

```
console.log(1+1);
console.log("2"+"4");
console.log(2+"4");
console.log(+new Date());
console.log(+"4");
```

作答如下

```
console.log(1+1); //2
console.log("2"+"4"); //24 有一个是字符串，转换为字符串的拼接。
console.log(2+"4"); //24
console.log(+new Date()); //1474781884386 获得当日的日期，用new Date() 参与计算会自动转换为从1970.1.1开始的毫秒数。
console.log(+"4"); //4 数值运算符+将字符串转换成数字。
```

## 3\. 以下代码的输出结果是?

```
var a = 1;
a+++a;

typeof a+2;
```

作答如下

```
var a = 1;
a+++a; //3

typeof a+2; //"number2"
```

在`a+++a`表达式当中，`a++`的优先级最高。所以相当于`(a++)+a`。`a`一开始赋值为`1`，`a++`表示先赋值再自增，所以`a++`的计算结果为`1`，且此时`a`等于`2`。所以`a+++a`表达式的计算结果为`3`。 而`typeof a + 2`中，`typeof`的优先级比`+`高，所以它会先计算`typeof a`，得到的输出是`"number"`。然后是一个字符串加上一个数字，会把数字转换成字符串。所以得到的输出为`"number2"`的字符串。

## 4\. 遍历数组，把数组里的打印数组每一项的平方

```
var arr = [3,4,5]
// todo..
// 输出 9, 16, 25
```

```
var arr = [3,4,5];
var i = 0;
while (i < arr.length){
  console.log (arr[i] * arr[i]);
  i++;
}
```

## 5\. 遍历JSON, 打印里面的值

```
var obj = {
  name: 'hunger',
  sex: 'male',
  age: 28
}
//todo ...
// 输出 name: hunger, sex: male, age:28
```

```
var obj = {
    name: 'hunger',
    sex: 'male',
    age: 28
};
     for (var i in obj) {
         console.log(i + '：' + obj["i"])
};
```

## 6\. 下面代码的输出是? 为什么

```
console.log(a);
var a = 1;
console.log(a);
console.log(b);
```

```
console.log(a); //输出undefined,因为变量a的变量提升
var a = 1; //声明一个变量`a`,并将数值1赋值于他
console.log(a); //输出1,因为a被赋值于1
console.log(b); //报错,因为变量b没有被声明
```
