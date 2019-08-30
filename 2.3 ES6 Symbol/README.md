##  概述

ES6 引入了一种新的原始数据类型 Symbol,表示独一无二的值，最大的用法是用来定义对象的唯一属性名。ES6 数据类型除了 `Number`、`String`、`Boolean`、`Object`、`null` 和 `undefined` , 还新增了 Symbol。

## 基本用法

`Symbol` 函数栈不能用 new 命令，因为 `Symbol` 是原始数据类型，不是对象。可以接受一个字符串作为参数，为新创建的 `Symbol` 提供描述，用来显示在控制台或者作为字符串的时候使用，便于区分。

```javascript
let sy = Symbol("kk");
console.log(sy); // Symbol(kk)
typeof(sy);      // "Symbol"

//相同参数 Symbol() 返回的值不相等
let sy1 = Symbol("kk");
sy === sy1;  // false
```

## 使用场景

### 作为属性名

**用法**

由于每一个 Symbol 的值都是不相等的，所以 Symbol作为对象的属性名，可以保证属性不重名

```javascript
let sy = Symbol("key1");

//写法1
let syObject = {};
syObject[sy] = "sk";
console.log(syObject);   //{ Symbol(key1) : "kk" }

//写法2
let syObjcet = {
    [sy]:"kk"
};
console.log(sysObject);  //{ Symbol(key1) : "kk" }

//写法3
let syObject = {};
Object.defineProperty(sysObjcet,sy,{value:"kk"})
console.log(syObject);  //{ symbol(key1) : "kk" }        
```

Symbol 作为对象属性名时不能用`.`运算符，要用方括号。因为`.`运算符后面是字符串，所以取到的是字符串sy属性,而不是Symbol的sy属性。

```javascript
let sy = Symbol("kk");
let syObjcet = {};
syObject[sy] = "kk";

syObjcet[sy]; //kk
syObjcet.sy;  //undefined
```

### 注意点

Symbol 值作为属性名时，该属性是公有的属性不是私有的属性，可以在类的外部访问。但是不会出现在`for...in`、`for...of`的循环中，也不会被 Object.keys()、Objcet.getOwnPropertyNames()返回。如果要读取到一个对象的 Symbol 属性，可以通过 Objcet.getOwnProperySymbols() 和 Reflect.ownKeys() 取到。

```javascript
let sy = Symbol("key1");

let syObjcet = {};
syObjcet[sy] = "kk";
console.log(syObjcet);

for(let i in syObjcet){
     console.log(i);  
}//无输出

Objcet.keys(syObjcet);                 // []
Objcet.getOwnPropertySymbol(syObject); // [Symbol(key1)]
Reflect.ownKeys(syObjcet);             // [Symbol(key1)]
```

### 定义常量

在 ES5 使用字符串表示常量。例如

```javascript
const COLOR_RED = "red";
const COLOR_YELLOW = "yellow";
const COLOR_BLUE = "blue";
```

但是用字符串不能保证常量是独特的，这样会引起一些问题：

```javascript
const COLOR_RED = "red";
const COLOR_YELLOW = "yellow";
const COLOR_BLUE = "blue";
const MY_BLUE = "blue"；
 
function getConstantName(color) {
    switch (color) {
        case COLOR_RED :
            return "COLOR_RED";
        case COLOR_YELLOW :
            return "COLOR_YELLOW ";
        case COLOR_BLUE:
            return "COLOR_BLUE";
        case MY_BLUE:
            return "MY_BLUE";         
        default:
            throw new Exception('Can't find this color');
    }
}
```

但是使用 Symbol 定义常量，这样就可以保证这一组常量的值都不相等。用 Symbol 来修改上面的例子。

```javascript
const COLOR_RED = Symbol("red");
const COLOR_YELLOW = Symbol("yellow");
const COLOR_BLUE = Symbol("blue");
 
function getConstantName(color) {
    switch (color) {
        case COLOR_RED :
            return "COLOR_RED";
        case COLOR_YELLOW :
            return "COLOR_YELLOW ";
        case COLOR_BLUE:
            return "COLOR_BLUE";
        default:
            throw new Exception('Can't find this color');
    }
}
```

Symbol 的值是唯一的，所以不会出现相同值得常量，即可以保证 switch 按照代码预想的方式执行。

### Symbol.for()

Symbol.for() 类似单例模式，首先会在全局搜索被登记的 Symbol 中是否有该字符串参数作为名称的 Symbol 值，如果有即返回该 Symbol 值，若没有则新建并返回一个以该字符串参数为名称的 Symbol 值，并登记在全局环境中供搜索。

```javascript
let yellow = Symbol("Yellow");
let yellow1 = Symbol.for("Yellow");
yellow === yellow1;      // false
 
let yellow2 = Symbol.for("Yellow");
yellow1 === yellow2;     // true
```

### Symbol.keyFor()

Symbol.keyFor() 返回一个已登记的 Symbol 类型值的 key ，用来检测该字符串参数作为名称的 Symbol 值是否已被登记。

```javascript
let yellow1 = Symbol.for("Yellow");
Symbol.keyFor(yellow1);    // "Yellow"
```

