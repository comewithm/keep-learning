### 语言基础

#### 变量
##### var关键字
1. var声明作用域

使用var操作符定义的变量会成为它的函数的局部变量。
```javascript
    function test() {
        var message = 'hi' // 局部变量
    }
    test()
    console.log(message) // 出错
```
若是去掉var，message就成了全局变量，并且可以在函数外部访问到。

定义多个变量，可以用逗号分隔
```javascript
    var message = 'hi',
        age = 20,
        flag = false;
```

2. var声明提升

**var关键字声明的变量会自动提升到函数作用域顶部**
```javascript
    function foo() {
        console.log(age)
        var age = 20
    }
    foo() // undefined

    // 等价于
    function foo() {
        var age
        console.log(age)
        age = 20
    }
    foo() // undefined
```
##### let 声明
> let声明的范围是块作用域,而var声明的是函数作用域。块级作用域是函数作用域的子集。
```javascript
    if(true) {
        var name = 'Matt'
        console.log(name) // Matt
    }
    console.log(name) // Matt

    if(true) {
        let age = 20
        console.log(age) // 20
    }
    console.log(age) // Reference Error: age 没有定义
```
###### 1. 暂停性死区
let与var另一个重要区别，就是let声明的变量不会在作用域中被提升。
```javascript
    // name会被提升
    console.log(name); // undefined
    var name = 'Matt'

    // age不会被提升
    console.log(age); // ReferenceError:age 没有定义
    let age = 20
```
###### 2.全局声明
与var关键字不同，使用let在全局作用域中声明的变量不会成为window对象的属性(var声明的变量则会)。
```javascript
    var name = 'Matt'
    console.log(window.name) // Matt

    let age = 20
    console.log(window.age) // undefined
```

##### const 声明
const行为与let基本相同，唯一区别是用它声明变量时必须同时初始化变量，且尝试修改const声明的变量会导致运行时错误
```javascript
    const age = 20
    age = 30 // TypeError: 给常量赋值
```
const声明的限制只适用于它指向的变量的引用，即如果const变量引用的是一个对象，那么修改这个对象内部的属性并不违反const的限制
```javascript
    const person = {}
    person.name = 'Matt' // ok
```
---
#### 数据类型
##### typeof操作符
对一个值使用typeof操作符会返回下列字符串之一
1. "undefined": 表示值未定义
2. "boolean": 表示值为布尔值
3. "string": 表示值为字符串
4. "number": 表示值为数值
5. "object": 表示值为对象(而不是函数) 或者 null
6. "function": 表示值为函数
7. "symbol": 表示值为符号

##### Undefined类型
注意: 包含undefined值得变量跟未定义变量是有区别的。
```javascript
    let message;

    // let age

    console.log(message); // "undefined"
    console.log(age); // 报错
```
但是，使用typeof时,两者返回的都是undefined。
```javascript
    let message;

    // let age

    console.log(typeof message); // "undefined"
    console.log(typeof age); // "undefined"
```
---
##### Null类型
逻辑上讲，null值表示一个空对象指针，这也是`typeof null == 'object'`的原因。
在定义将来要保存对象值的变量时,建议使用null来初始化，不要用其他值，只要检查该变量是不是null就可以知道这个变量是否在后来被重新赋予了一个对象的引用。

---
##### Boolean类型
不同类型值与布尔值之间的转换

| 数据类型 | 转换为true的值 | 转换为false的值 | 
| ---------- | :----------: | :----------: |
| Boolean | true | false |
| String | 非空字符串 | ""(空字符串) |
| Number | 非零数值(包括无穷值) | 0,NaN |
| Object | 任意对象 | null |
| Undefined | N/A(不存在) | undefined |

##### Number类型
###### 值的范围
ECMAScript可以标识的最小数值保存在Number.MIN_VALUE中,这个值在多数浏览器中是：5e-324。最大数值保存在Number.MIN_VALUE中,这个值在多数浏览器中是1.797 693 134 862 315 7e+308。超出范围分别用-Infinity 和 Infinity表示。
使用`isFinite()`函数可以确定一个值是不是有限大。

###### NaN
有一个特殊的数值叫NaN,意思是"不是数值"(Not a Number),用来表示本来要返回数值的操作失败了(而不是抛出错误)
```javascript
    console.log(0 / 0) // NaN
    console.log(+0 / -0) // NaN
```
任何涉及NaN的操作始终返回NaN(如NaN/10)。NaN不等于包括NaN在内的任何值。
```javascript
    console.log(NaN == NaN) // false
```
ECMAScript提供了`isNaN()`函数。该函数接收一个参数，可以是任意数据类型。
```javascript
    console.log(isNaN(NaN)) // true
    console.log(isNaN(10)) // false, 10是数值
    console.log(isNaN('10')) // false 可以转换为数值10
    console.log(isNaN('blue')) // true,不可以转换为数值
    console.log(isNaN(true)) // false, 可以转换为数值1
```
###### 数值转换
1. Number()函数转换规则
> boolean值, true转换为1, false转换为0
> null, 返回0
> undefined, 返回NaN
> string, 空字符串返回0。包含数值字符(包括前面带加减符号)则转换为十进制数。
> 对象, 调用valueOf()方法,按照上述规则转换返回的值。如果转换结果是NaN,则调用toString()方法,再按照转换字符串规则转换。
2. parseInt()函数
> 更专注与字符串是否包含数值模式。字符串最前面的空格会被忽略,从第一个非空格字符开始转换。如果不是数值字符、加好或减号, parseInt()返回NaN。这意味着空字符串也会返回NaN(Number(" ")返回0)
```javascript
    let num1 = parseInt('1234blue') // 1234 blue
    let num2 = parseInt(" ") // NaN
    let num3 = parseInt("0xA") // 10 十六进制数
    let num4 = parseInt(22.5) // 22
```
parseInt()接收第二个参数,用于指定进制数
```javascript
    let num1 = parseInt("AF", 16) // 175
    let num2 = parseInt("AF") // NaN
```
3. parseFloat()
从位置0开始检测每个字符,它也是解析到字符串末尾或者解析到一个无效的浮点数值字符为止。
另一个不同之处是，它始终忽略字符串开头的零。parseFloat()只解析十进制数,因此不能指定底数。
```javascript
    let num1 = parseFloat('1234blue') // 1234
    let num2 = parseFloat('0xA') // 0
    let num3 = parseFloat('22.5') // 22.5
    let num4 = parseFloat('22.24.5') // 22.24
    let num5 = parseFloat('0908.5') // 908.5
    let num6 = parseFloat('3.125e7') // 31250000
```

---
##### String类型
##### 转换为字符串
有两种方式将一个值转换为字符串

`toString()`方法可见于数值，布尔值，对象和字符串值。(字符串值也有toString()方法，该方法只是简单的返回自身的一个副本)。null和undefined没有toString()方法。

多数情况下,toString()不接收任何参数。但在对数值调用该方法时,toString()可以接收一个底数,表示以什么底数来输出数值的字符串表示。

null和undefined没有toString()方法，String()方法直接返回了这两个值的字面量文本。
```javascript
    let num = 10
    console.log(num.toString(8)) // "12"
    console.log(num.toString(16)) // "a"

    let value1 = 10
    let value2 = true
    let value3 = null
    let value4
    console.log(String(value1)) // "10"
    console.log(String(value2)) // "true"
    console.log(String(value3)) // "null"
    console.log(String(value4)) // "undefined"
```
