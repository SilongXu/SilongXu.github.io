---
title: Typescript notes
abbrlink: 'typescriptNotes'
categories: typescript
tags: [typescript]
---

# Typescript学习笔记



```javascript
const button = document.querySelector("button");
const input1 = document.getElementById("num1");
const input2 = document.getElementById("num2");

function add(num1, num2){
    return num1 + num2;
}

button.addEventListener("click", function()) {
	console.log(add(input1.value, input2.value));                        
}
//在javascript中，无论你在input框设置type为什么，拿到的都视为string，所以这个相加会看作为两个string相加
//在typescript中就可以得到15
```

Typescript的本质： typescript是不能在浏览器上直接运行的，通常我们使用typescript进行代码编写，是因为他能更好的，更安全的编写代码。为了让我们用ts写的代码可以在浏览器上运行，就需要将其转为js文件.  通常命令为 tsc  abc.ts

## Installing and Using

Using npm to download Typescript ： `npm install -g typescript`

typescript file 后缀名是 .ts



```typescript
const button = document.querySelector("button");
const input1 = document.getElementById("num1") ! as HTMLInputElement; //加上！后面的内容告知TS input1是一个HTML的标签，是确实存在的，不然在input1.value那里会报错
const input2 = document.getElementById("num2") ! as HTMLInputElement;

function add(num1: number, num2: number){//如果不限制type那么就会默认为any
    return num1 + num2;
}

button.addEventListener("click", function()) {
	console.log(add(+input1.value, +input2.value));                        
}
//在javascript中，无论你在input框设置type为什么，拿到的都视为string，所以这个相加会看作为两个string相加
//在typescript中就可以得到15
```

然后将typescript文件通过tsc 命令转为js, 得到：

```javascript
const button = document.querySelector("button");
const input1 = document.getElementById("num1");
const input2 = document.getElementById("num2");

function add(num1, num2){
    return num1 + num2;
}

button.addEventListener("click", function()) {
	console.log(add(+input1.value, +input2.value));                        
}
//他可以很好的处理10+5这个计算，得到15 而不是105
```



## Advantage os Typescript

Types!

Next-gen JavaScript Features (compiled down for older Browsers)

Non-JavaScriopt Features like Interfaces or Generics

Meta-Programming Features like Decorators

Rich Configuration Options

Modern Tooling that helps even in non-TypeScript Projects

## Setting Developpement Environment

IDE: Vs-code

Extension: ESLint, Material Icon Theme, Path Intellisense, Prettier - Code formatter, TSLint

## Types

Typescript支持用户构建自己的数据类型

Javascript type和Typescript type区别：key difference is: JS uses "dynamic types"(resolved at runtime), TS uses "static types"(set during development)

### Core Types

| Type    | Instance        | Description                                                  |
| ------- | --------------- | ------------------------------------------------------------ |
| number  | 1, 5.3, -10     | All numbers, no differentiation between integers or floats   |
| string  | 'Hi', "Hi",     | All text values                                              |
| boolean | true, false     | Just these two, no "truthy" or "falsy" values                |
| object  | {age:30}        | Any javascript object,more specific types(type of object) are possible |
| Array   | [1,2,3]         | Any JavaScript array, type can be flexible or strict(regarding the element tyeps) |
| --Tuple | [1,2]           | Added by TypeScript: Fixed-length array                      |
| --Enum  | enum {NEW, OLD} | Added by TypeScript: Automatically enumerated global constrant identifiers |
| --Any   | *               | Any kind of value, no specific type assignment               |

typescript在函数的参数声明的时候，以及变量的声明的时候 需要带上数据类型，像java一样。如果不带会默认为any

typescript常用typeof方法来判断变量的数据类型然后代码再继续下一步。`typeof n1 !==  'number'`

#### Object Types

```typescript
const person: {
    name: string;//注意这里不是逗号是分号
    age: number;
} = {
    name: 'Maximilian',
    age: 30
}
```

#### Array Types

```typescript
const person = {
    name: "silong",
    age: 23,
    hobbies: ['Sports','Cooking']
};

let favoriteActivities: string[];
favoriteActivities = ['Sports'];

for (const hobby of person.hobbies){
    console.log(hobby);
    //hobby.map  ERROR
}
```

#### Tuple

```typescript
const person: {
    name: string;//注意这里不是逗号是分号
    age: number;
    role: [number, string];//an array accept 2 types, first value is a number, second is a string
} = {
    name: 'Maximilian',
    age: 30,
    role: [2,'author'] // Tuple
}

person.role.push('admin')//it works

person.role = [0, 'admin']//it works
person.role = [0,'admin','user'] //error
```

#### Enum

Important: Often, you'll see enums with all-uppercase values but that's not a "must-do". You can go with ANY value names

和JAVA中的 枚举 类似

```
enum Role {ADMIN = 5, READ_ONLY, AUTHOR} //READ_ONLY会变成6

const person = {
	name: "silong",
	age:30,
	role: Role.ADMIN
}

if (person.role === Role.AUTHOR){
	...
}
```

#### Union Type

关键符号   |

```typescript
function comine(input1: number | string, input2: number | string){ //可以让输入的参数接收两个或多个数据类型
	let result;
    if(typoof input1 === 'number' && typeof input2 === 'number'){
        result = input1 + input2;
    }else{
        result = input1.toString() + input2.toString();
    }
    return result;
}
```

#### Literal Type

```typescript

function comine(input1: number | string, input2: number | string,
                resultConversion: 'as-number' | 'as-text'){ //resultConversion只能接收这两个值
	let result;
    if(typoof input1 === 'number' && typeof input2 === 'number' || resultConversion === 'as-number'){//必须得和上面两个值的其中一个一致不然会报错
        result = input1 + input2;
    }else{
        result = input1.toString() + input2.toString();
    }
    return result;
}
```

#### Type alias. important

```typescript
//使用type关键字 将 union Type 简化为一个自己命名的type
type combinable = number | string ;
type conversionDescriptor = 'as-number' | 'as-text';

function comine(input1: combinable, input2: combinable,
                resultConversion: conversionDescriptor){ //resultConversion只能接收这两个值
	let result;
    if(typoof input1 === 'number' && typeof input2 === 'number' || resultConversion === 'as-number'){//必须得和上面两个值的其中一个一致不然会报错
        result = input1 + input2;
    }else{
        result = input1.toString() + input2.toString();
    }
    return result;
}
```

### Function

#### Function return type and Void

Function return type如果不指明那就是根据变量类型和return类型自适应

```typescript
function add (n1: number, n2: number): void{
	return n1 + n2;
}
```

#### Function type and callbacks

```
function add (n1: number, n2: number){
	return n1 + n2;
}
function printResult(num: number): void{
	console.log("Result:" + num);
}

let combineValues: Function;
combineValues = add;
combineValues = printResult; // combineValues会被认为是printResult而不是add

//因此需要更精确一点.使用箭头函数
let combineValue2: (a:number, b:number) => number;
combineValue2 = add;//works
combineValue2 = printResult//error


//callbacks
function addAndHanle(n1:number, n2:number,cb:(num: number) => void){
	const result = n1+n2;
	cb(result);
}

addAndHandle(10,20,(result) => {
	console.log(result);//会显示30
})
```

### The other type

#### Unknown type

unknown是一个类型安全的any。因为如果使用了any就不能进行类型判断了所以通常在想使用any的室友用unknown代替

任何类型的值都可以赋给 `unknown` 类型，但是 `unknown` 类型的值只能赋给 `unknown` 本身和 `any` 类型。

```
let userInput: unknown;
let userName: string

userInput =5;
userInput = 'Max';
if(typeof userInput === 'string'){
	userName = userInput;
}
```

#### Never type



```
let userInput: unknown;
let userName: string

userInput =5;
userInput = 'Max';
if(typeof userInput === 'string'){
	userName = userInput;
}

function generateError(message: string, code:number): never{
	throw {message: message, errorCode: code};
}

cont result = generateError('An error occurred!', 500);
console.log(result);//会显示空白而不是undefined,因为throws中断了代码的进行。这里function的返回值是never 类型
```

#### 