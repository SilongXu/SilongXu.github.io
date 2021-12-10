---
title: ES6复习总结 part1
categories: JavaScript
tags: [es6,javascript]
abbrlink: 'reviewEs6Part1'
---

# ES 2015(ES6) 复习总结 part1

## let const关键词

### let

let关键词可以在一个block内声明一个变量并给其赋值

```javascript
var x = 10;
// x在这里值为10
{
    let x = 2;
    //x在这里值为2
}
//x在这里值为10
```

### const

const关键词可以创建一个变量并且给其赋值。它和let相似（block内），但是赋值之后就不能修改。并且创建的时候必须初始化

```javascript
var x = 10;
// x在这里值为10
{
    const x = 2;
    //x在这里值为2
}
//x在这里值为10
```

**Tips** const 定义的对象属性是可以改变的。因为对象是引用类型的，person中奥村的仅仅是对象的指针，这就说明const仅保证指针不发生改变，修改对象的属性并不会改变对象的指针所以是被允许的。也就是说const定义的引用类型只要指针不发生改变，其他的不论如何改变都是允许的。

```javascript
//可行
const person ={
	name: "julien",
	sex: "male"
}
person.name = "alex"

//不可行
const person ={
	name: "julien",
	sex: "male"
}

person = {
	name:"annie",
	sex: "female"
}
```



### var let const的区别 *

#### 变量提升

JS中，函数以及变量的声明都将被提升到函数的最顶部。

JS中，变量可以在使用后声明，也就是变量可以先使用再声明

#### 主要区别

`let作用于块级作用域，在块定义使用let声明变量对块外部无影响。let声明的变量可以改变（指针），值和类型都可以改变，没有限制。let不能在定义之前访问该变量，这么做会报错。let定义过的变量在同一个块内不能重新定义`  

`const作用于块级作用域，在块定义使用const声明变量对 块外部 无影响。const声明变量时必须初始化，不能更改其指针，类型，但是可以修改const声明的对象的属性的值`。

`var作用于函数作用域。存在变量提升。var可以在定义之前访问该变量，该变量会显示undefined。var定义过的变量在同一块作用域内可以重新定义，值为最新定义的值。`

## arrow Functions  箭头函数

[JS箭头函数介绍-简书](https://www.jianshu.com/p/f853d6a7b548) 

更简便的书写function表达式

箭头函数不需要 function 关键词，不强制需要return关键词,以及大括号。

使用const声明箭头函数比var要更好

```javascript
// ES6
const x = (x, y) => x * y;

const x = (x, y) => { return x * y };
```

箭头函数没有自己的this, arguments, super 或new.target

## For / of 循环

[MDN文档描述](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) 

for.....of语句在可迭代对昂（Array,Map,Set,String,TypeArray,arguments对象等等) 上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句

```javascript
const array1 = ['a', 'b', 'c'];

for (const element of array1) {
  console.log(element);
}

// expected output: "a"
// expected output: "b"
// expected output: "c"

//语法
for (variale of iterable){
    //statements
}
//variale:在每次迭代中，将不同属性的值分配给变量
//iterable:被迭代枚举其属性的对象


let iterable = [10, 20, 30];
for (let value of iterable) {
    value += 1;
    console.log(value);
}
// 11
// 21
// 31

//如果你不想修改语句块中的变量 , 也可以使用const代替let。
let iterable = [10, 20, 30];

for (const value of iterable) {
  console.log(value);
}
// 10
// 20
// 30


//迭代String
let iterable = "boo";
for (let value of iterable) {
  console.log(value);
}
// "b"
// "o"
// "o"

//迭代Map
let iterable = new Map([["a", 1], ["b", 2], ["c", 3]]);

for (let entry of iterable) {
  console.log(entry);
}
// ["a", 1]
// ["b", 2]
// ["c", 3]

for (let [key, value] of iterable) {
  console.log(value);
}
// 1
// 2
// 3


//迭代Set
let iterable = new Set([1, 1, 2, 2, 3, 3]);

for (let value of iterable) {
  console.log(value);
}
// 1
// 2
// 3
```

## Map 对象

[MDN文档描述](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) 

Being able to use an Object as a key is an important Map feature

Map对象保存键值对，并且能够记住键的原始插入顺序。任何值都可以作为一个键或者一个值

[Map常用方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) 

## Set 对象

[MDN文档描述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set) （包含基本操作合集）

set对象是值的集合，可以按照插入的顺序迭代它的元素。Set中的元素只会出现一次，即set中的元素是唯一的。

## Classes 类

[MDN描述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/class) 

JS中的类对象是用来创建对象的一个模板

跟Java的使用方法差不多，有个构造器，也可以设置getter,setter etc

在这里就不描述了，因为对class了解足够。

## Promise 异步函数

[如何使用promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises) 

[promise介绍](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 

看这两个基本就够了

## Symbol 数据类型

Symbol是一种基本数据类型。具有静态属性和静态方法。

静态属性会暴露几个内建的成员对象；静态方法会暴露全局的symbol注册，且类似于内建对象类，但是作为构造函数来说不完整，所以不支持语法 new Symbol（）。

Symbol 本质上是一种唯一标识符，可用作对象的唯一属性名。



[MDN对Symbol的描述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 

[CSDN上某个博主对Symbol常用方法的列举](https://blog.csdn.net/qq_33408245/article/details/82953143) 

### 基本使用方法

```javascript
var sym1 = Symbol('foo');
//Symbol数据类型的特点是唯一性，即使是用同一个变量生成的值也不相等
let id1 = Symbol('id');
let id2 = Symbol('id');
console.log(id1 == id2)  //false

//不能使用new Symbol()创建包装器对象，但是可以使用Object()函数将其创建为一个包装器对象
var sym = Symbol("foo");
typeof sym; // "symbol"
var symObj = Object(sym);
typeof symObj; //"object"

//上面的方法创建的symbol对象只能在局部访问的到。要想在整个代码库里面都能找到创建的symbol类型对象，需要用到Symbol.for()和Symbol.keyFor()
//Symbol.for() 检测对应描述的symbol对象是否已存在
let name1 = Symbol.for('name'); //检测到尚未创建 --- 新建
let name2 = Symbol.for('name'); //检测到已经创建 --- 返回
console.log(name1 === name2) //true
//Symbol.keyFor() 用于通过symbol对象获取参数值
let name1 = Symbol.for('name'); 
let name2 = Symbol.for('name');
console.log(Symbol.keyFor(name1)); // 'name'
console.log(Symbol.keyFor(name2)); // 'name'


//Symbols无法在for...in中枚举。但是可以使用Object.getOwnPropertySymbols()得到
var obj ={};
obj[Symbol("a")] = "a";
obj[Symbol.for('b')] = 'b';
obj["c"] = "c";
obj.d = "d";

for (var i in obj) {
   console.log(i); // logs "c" and "d"
}
let array = Object.getOwnPropertySymbols(obj);
console.log(array); //[Symbol("a"),Symbol("b")]
console.log(obj[array[0]]); //"a"

//当使用JSON.stringify时，以symbol值作为键的属性会被完全忽略
JSON.stringify({[Symbol("foo")]: "foo"});
// '{}'

//当一个 Symbol 包装器对象作为一个属性的键时，这个对象将被强制转换为它包装过的 symbol 值
var sym = Symbol("foo");
var obj = {[sym]: 1};
obj[sym];            // 1
obj[Object(sym)];    // still 1
```



## Default Parameters 默认参数

[MDN描述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Default_parameters) 

简单理解就是： 函数默认参数允许在没有值或者undefined被传入时使用默认形参

```javascript
//普通函数。没有提供参数b的值的时候
//
function multiply(a,b){
    return a * b;
}
multiply(5,2); //10
multiply(5); //NaN

//为了防止这种情况，可以使用下面的代码解决问题（在默认参数出现之前的方法）
//
function multiply(a, b) {
  b = (typeof b !== 'undefined') ?  b : 1;
  return a * b;
}

multiply(5, 2); // 10
multiply(5);    // 5

//有了默认参数之后
function multiply(a, b = 1) {
  return a * b;
}

multiply(5, 2); // 10
multiply(5);    // 5

//传入undefined vs 其他假值
//
function test(num = 1) {
  console.log(typeof num);
}
test();          // 'number' (num is set to 1)
test(undefined); // 'number' (num is set to 1 too)
// test with other falsy values:
test('');        // 'string' (num is set to '')
test(null);      // 'object' (num is set to null)


//默认参数可用于后面的默认参数
//
function greet(name, greeting, message = greeting + ' ' + name) {
    return [name, greeting, message];
}
greet('David', 'Hi');  // ["David", "Hi", "Hi David"]
greet('David', 'Hi', 'Happy Birthday!');  // ["David", "Hi", "Happy Birthday!"]

//结构参数也一样适用
//
function f([x, y] = [1, 2], {z: z} = {z: 3}) {
  return x + y + z;
}
f(); // 6
```



## Function Rest Parameter 剩余参数

[MDN描述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Rest_parameters)

如果函数的最后一个命名参数以 ... 为前缀， 则它将成为一个由剩余参数组成的真数组，其中从0（包括） 到theArgs.length（排除）的元素由传递给函数的实际参数提供

### 剩余参数和arguments对象的区别

#### arguments对象

[MDN描述 ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments)

- arguments是一个对应于传递给函数的参数的类数组对象

- arguments对象是所有（非箭头）函数中都可用的局部变量。

- 可以使用arguments对象在函数中引用函数的参数，此对象包含传递给函数的每个参数，第一个 参数在索引0，以此类推。如果一个函数传递了三个参数，可以这么引用： `arguments[0], arguments[1],arguments[2]` 

- 参数也可以被设置： arguments[1] = "new value";

- arguments对象不是一个array。类似Array,但除了length属性和索引元素之外没有任何Array属性
- arguments.callee:指向参数所属的当前执行的函数；指向调用当前函数的函数。
- arguments.length: 传递给函数的参数数量
- arguments.[@@iterator] 返回一个新的Array迭代器 对象，该对象包含参数中每个索引的值

```javascript
//例子
function add() {
    var sum =0,
        len = arguments.length;
    for(var i=0; i<len; i++){
        sum += arguments[i];
    }
    return sum;
}
add()                           // 0
add(1)                          // 1
add(1,2,3,4);                   // 10
```

#### 区别

剩余参数和 `arguments`对象之间的区别主要有三个：

- 剩余参数只包含那些没有对应形参的实参，而 `arguments` 对象包含了传给函数的所有实参。

- `arguments`对象不是一个真正的数组，而剩余参数是真正的 `Array`实例，也就是说你能够在它上面直接使用所有的数组方法，比如 `sort`，`map`，`forEach`或`pop`。 为了在arguments对象上使用Array方法，它必须首先被转换为一个真正的数组

  ```javascript
  function sortArguments() {
    var args = Array.prototype.slice.call(arguments);
    var sortedArgs = args.sort();
    return sortedArgs;
  }
  console.log(sortArguments(5, 3, 7, 1)); // shows 1, 3, 5, 7
  ```

- `arguments`对象还有一些附加的属性 （如`callee`属性）。

