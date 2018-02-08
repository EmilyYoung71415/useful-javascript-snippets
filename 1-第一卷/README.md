# 说明
>知识点来自以下两个链接，我只是做了点归纳[选了自己感兴趣的]，换了个排版而已

+ [这些JavaScript编程黑科技，装逼指南，高逼格代码，让你惊叹不已](https://github.com/jawil/blog/issues/24)
+ [JavaScript 优雅的实现方式包含你可能不知道的知识点](https://github.com/jawil/blog/issues/30)



# 第一卷目录

* [`数组去重`](#📚数组去重)
* [`数字格式化千分符 1234567890 --> 1,234,567,890`](#📚数字格式化千分符)
* [`交换两个整数`](#📚交换两个整数)
* [`将argruments对象转换成数组`](#📚将argruments对象转换成数组)
* [`数字取整2.33333=>2`](#📚数字向下取整)
* [`数组求和`](#📚数组求和)
* [`单行写评级组件`](#📚单行写评级组件)
* [`优雅的取随机字符串`](#📚优雅的取随机字符串)
* [`实现标准JSON的深拷贝`](#📚深拷贝)
* [`不用Number、parseInt和parseFloat和方法把"1"字符串转换成数字`](#📚1字符串转换成数字)
* [`数组中最大值和最小值`](#📚数组中最大值和最小值)
* [`数组查找O(n)算法`](#📚数组查找)



# 实例

## 📚数组去重

[⬆ Back to top](#第一卷目录)

注：暂不考虑对象字面量，函数等引用类型的去重，也不考虑 NaN, undefined, null等特殊类型情况。
>数组样本：[1, 1, '1', '2', 1]

### 1.数组去重普通版
+ 思路
>无需思考，我们可以得到 O(n^2) 复杂度的解法。定义一个变量数组 res 保存结果，遍历需要去重的数组，如果该元素已经存在在 res 中了，则说明是重复的元素，如果没有，则放入 res 中。

+ 优缺点比较

|优点|没有任何兼容性问题，通俗易懂，没有任何理解成本|
|--|--|
|缺点|看起来比较臃肿比较繁琐，时间复杂度比较高O(n^2)|

<details>
<summary>普通版代码实例👈👈👈</summary>

```javascript
var a = [1, 1, '1', '2', 1]
function unique(arr) {
    var res = []
    for (var i = 0, len = arr.length; i < len; i++) {
        var item = arr[i]
        for (var j = 0, len = res.length; j < jlen; j++) {
            if (item === res[j]) //arr数组的item在res已经存在,就跳出循环
                break
        }
        if (j === jlen) //循环完毕,arr数组的item在res找不到,就push到res数组中
            res.push(item)
    }
    return res
}
console.log(unique(a)) // [1, 2, "1"]
```
</details>

---

### 2.数组去重进阶版
+ 思路
>数组filter，return array.indexOf(ele) === index

+ 优缺点比较

|优点|很简洁，思维也比较巧妙，直观易懂|
|--|--|
|缺点|不支持 IE9 以下的浏览器，时间复杂度还是O(n^2)|

<details>
<summary>进阶版代码实例👈👈👈</summary>

```javascript
var a =  [1, 1, '1', '2', 1]
function unique(arr) {
    return arr.filter(function(ele,index,array){
        return array.indexOf(ele) === index//很巧妙,这样筛选一对一的,过滤掉重复的
    })
}
console.log(unique(a)) // [1, 2, "1"]
```
</details>

---

### 3.数组去重进阶优化版
+ 思路
>数组filter，obj.hasOwnProperty

+ 优缺点比较

|优点|1. hasOwnProperty 是对象的属性(名称)存在性检查方法。对象的属性可以基于 Hash 表实现，因此对属性进行访问的时间复杂度可以达到O(1);<br/>2. filter 是数组迭代的方法，内部还是一个 for 循环，所以时间复杂度是 O(n)|
|--|--|
|缺点|不兼容 IE9 以下浏览器，其实也好解决，把 filter 方法用 for 循环代替或者自己模拟一个 filter 方法。|

<details>
<summary>进阶优化版代码实例👈👈👈</summary>

```javascript
var a =  [1, 1, '1', '2', 1]
function unique(arr) {
    var obj = {}
    return arr.filter(function(item, index, array){
        return obj.hasOwnProperty(typeof item + item) ? 
        false : 
        (obj[typeof item + item] = true)
    })
}

console.log(unique(a)) // [1, 2, "1"]
```
</details>

---

### 4.数组去重终极版
+ 思路
>以 Set 为例，ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

+ 优缺点比较

|优点|ES6 语法，简洁高效，我们可以看到，去重方法从原始的 14 行代码到 ES6 的 1 行代码，其实也说明了 JavaScript 这门语言在不停的进步，相信以后的开发也会越来越高效。|
|--|--|
|缺点|兼容性问题，现代浏览器才支持，有 babel 这些都不是问题。。|

<details>
<summary>终结版代码实例👈👈👈</summary>

```javascript
const unique = a => [...new Set(a)]
```
</details>

---


## 📚数字格式化千分符

[⬆ Back to top](#第一卷目录)

1234567890 --> 1,234,567,890

### 1.数字格式化千分符普通版
+ 思路
>空数组，如果是不是3的倍数就另外追加到上去 str.length % 3 && arr.unshift(str.slice(0, str.length % 3))。

+ 优缺点比较

|优点|比网上写的一堆 for循环 还有 if-else 判断的逻辑更加清晰直白。|
|--|--|
|缺点|太普通|

<details>
<summary>普通版代码实例👈👈👈</summary>

```javascript
function formatNumber(str) {
  let arr = [],
    count = str.length

  while (count >= 3) {
    arr.unshift(str.slice(count - 3, count))
    count -= 3
  }

  // 如果是不是3的倍数就另外追加到上去
  str.length % 3 && arr.unshift(str.slice(0, str.length % 3))

  return arr.toString()

}
console.log(formatNumber("1234567890")) // 1,234,567,890
```
</details>

---

### 2.数字格式化千分符进阶版

[⬆ Back to top](#第一卷目录)

+ 思路
>str.split("").reverse().reduce((prev, next, index) ，充分利用js的api

+ 优缺点比较

|优点|把 JS 的 API 玩的了如指掌|
|--|--|
|缺点|可能没那么好懂，不过读懂之后就会发出我怎么没想到的感觉|

<details>
<summary>进阶版代码实例👈👈👈</summary>

```javascript
function formatNumber(str) {

  // ["0", "9", "8", "7", "6", "5", "4", "3", "2", "1"]
  return str.split("").reverse().reduce((prev, next, index) => {
    return ((index % 3) ? next : (next + ',')) + prev
  })
}

console.log(formatNumber("1234567890")) // 1,234,567,890
```
</details>

---

### 3.数字格式化千分符Api版
+ 思路
>toLocaleString的妙用

+ 优缺点比较

|优点|简单粗暴，直接调用 API|
|--|--|
|缺点|Intl兼容性不太好，不过 toLocaleString的话 IE6 都支持|

<details>
<summary>Api版代码实例👈👈👈</summary>

```javascript
(123456789).toLocaleString('en-US')  // 1,234,567,890


//还可以使用 Intl对象
/*
    Intl 对象是 ECMAScript 国际化 API 的一个命名空间
    它提供了精确的字符串对比，数字格式化，日期和时间格式化。
    Collator，NumberFormat 和 DateTimeFormat 对象的构造函数
    是 Intl 对象的属性。
*/
new Intl.NumberFormat().format(1234567890) // 1,234,567,890
```
</details>


## 📚交换两个整数

[⬆ Back to top](#第一卷目录)

let a = 3,b = 4 变成 a = 4, b = 3

### 1.普通版
+ 思路
>最常规的办法，引入一个 temp 中间变量

+ 优缺点比较

|优点|一眼能看懂就是最好的优点|
|--|--|
|缺点|硬说缺点就是引入了一个多余的变量|

<details>
<summary>代码实例👈👈👈</summary>

```javascript
let a = 3,b = 4
let temp = a
a = b
b = temp
console.log(a, b)
```
</details>

---
### 2.进阶版
+ 思路
>在不引入中间变量的情况下也能交互两个变量 【运用+ -】

+ 优缺点比较

|优点|比起楼上那种没有引入多余的变量，比上面那一种稍微巧妙一点|
|--|--|
|缺点|当然缺点也很明显，整型数据溢出，比如说对于32位字符最大表示有符号数字是2147483647，也就是Math.pow(2,31)-1，如果是2147483645和2147483646交换就失败了。|

<details>
<summary>代码实例👈👈👈</summary>

```javascript
let a = 3,b = 4
a += b
b = a - b
a -= b
console.log(a, b)
```
</details>

---

### 3.终极版
+ 思路
>利用一个数异或本身等于０和异或运算符合交换律、位操作

+ 优缺点比较

|优点|不存在引入中间变量，不存在整数溢出|
|--|--|
|缺点|前端对位操作这一块可能了解不深，不容易理解|

<details>
<summary>代码实例👈👈👈</summary>

```javascript
let a = 3,b = 4
  a ^= b
  b ^= a
  a ^= b
console.log(a, b)

//解释：
a = 011
(^) b = 100
则  a = 111(a ^ b的结果赋值给a，a已变成了7)
(^) b = 100
则  b = 011(b^a的结果赋给b，b已经变成了3)
(^) a = 111
则  a = 100(a^b的结果赋给a，a已经变成了4)
/*
    1.对于开始的两个赋值语句，a = a ^ b，b = b ^ a，
    相当于b = b ^ (a ^ b) = a ^ b ^ b，而b ^ b 显然等于0。
    因此b = a ^ 0，显然结果为a。
    2. 同理可以分析第三个赋值语句，a = a ^ b = (a ^ b) ^ a = b

    ^ 即”异或“运算符。
    它的意思是判断两个相应的位值是否为”异“，为”异"(值不同)就取真（1）;否则为假（0）。

    ^ 运算符的特点是与0异或，保持原值；与本身异或，结果为0。
*/
```
</details>

---

### 4.ES6终终极版
+ 思路
>解构赋值
<details>
<summary>代码实例👈👈👈</summary>

```javascript
var a = 3,b = 4;
[b, a] = [a, b]
```
</details>

## 📚将argruments对象转换成数组

[⬆ Back to top](#第一卷目录)

{0:1,1:2,2:3,length:3}这种形式的就属于类数组，就是按照数组下标排序的对象，还有一个 length属性，有时候我们需要这种对象能调用数组下的一个方法，这时候就需要把把类数组转化成真正的数组。

### 1.普通版
+ 思路
>数组 依次判断 返回新数组

+ 优缺点比较

|优点|通用版本，没有任何兼容性问题|
|--|--|
|缺点|太普通|

<details>
<summary>代码实例👈👈👈</summary>

```javascript
var makeArray = function(array) {
  var ret = []
  if (array != null) {
    var i = array.length
    if (i == null || typeof array === "string") ret[0] = array
    else while (i) ret[--i] = array[i];
  }
  return ret
}
makeArray({0:1,1:2,2:3,length:3}) //[1,2,3]
```
</details>

---

### 2.进阶版
+ 思路
>call(arguments)

+ 优缺点比较

|优点|最常用的版本，兼容性较强|
|--|--|
|缺点|ie 低版本，无法处理 dom 集合的 slice call 转数组。（虽然具有数值键值、length 符合ArrayLike 的定义，却报错）搜索资料得到 ：因为 ie 下的 dom 对象是以 com 对象的形式实现的，js 对象与com对象不能进行转换 。|

<details>
<summary>代码实例👈👈👈</summary>

```javascript
var arr = Array.prototype.slice.call(arguments);

/*
    slice.call 的作用原理就是，利用 call，将 slice 的方法作用于 arrayLike，slice 的两个参数为空，slice 内部解析使得 arguments.lengt 等于0的时候
    相当于处理 slice(0) ： 即选择整个数组，slice 方法内部没有强制判断必须是 Array 类型，slice 返回的是新建的数组（使用循环取值）”，所以这样就实现了
    类数组到数组的转化，call 这个神奇的方法、slice 的处理缺一不可。
*/
```
</details>

---

### 3.ES6版本
+ 思路
>Array.from、扩展符

+ 优缺点比较

|优点|直接使用内置 API，简单易维护|
|--|--|
|缺点|兼容性，使用 babel 的 profill 转化可能使代码变多，文件包变大|

<details>
<summary>代码实例👈👈👈</summary>

```javascript
//使用 Array.from, 值需要对象有 length 属性, 就可以转换成数组
var arr = Array.from(arguments);

//或者使用扩展符
var args = [...arguments];
```
</details>

---

## 📚数字向下取整

[⬆ Back to top](#第一卷目录)

### 1.普通版
+ 思路
> parseInt

<details>
<summary>代码实例👈👈👈</summary>

```javascript
const a = parseInt(2.33333)
```
</details>

---

### 2.进阶版
+ 思路
> Math.trunc

<details>
<summary>代码实例👈👈👈</summary>

```javascript
const a = Math.trunc(2.33333)

//Math.trunc() 方法会将数字的小数部分去掉，只保留整数部分。
//特别要注意的是：Internet Explorer 不支持这个方法，写个Polyfill如下
Math.trunc = Math.trunc || function(x) {
  if (isNaN(x)) {
    return NaN;
  }
  if (x > 0) {
    return Math.floor(x);
  }
  return Math.ceil(x);
};

```
</details>

---


### 3.黑科技~~版

[⬆ Back to top](#第一卷目录)

+ 思路
> 双波浪线 ~~ 操作符也被称为“双按位非”操作符。你通常可以使用它作为代替 Math.trunc() 的更快的方法。

<details>
<summary>代码实例👈👈👈</summary>

```javascript
console.log(~~47.11)  // -> 47
console.log(~~1.9999) // -> 1
console.log(~~3)      // -> 3
console.log(~~[])     // -> 0
console.log(~~NaN)    // -> 0
console.log(~~null)   // -> 0

//失败时返回0,这可能在解决 Math.trunc() 转换错误返回 NaN 时是一个很好的替代。
//但是当数字范围超出 ±2^31−1 即：2147483647 时，异常就出现了：

// 异常情况
console.log(~~2147493647.123) // -> -2147473649 🙁
```
</details>

---

### 3.黑科技number | 0版
+ 思路
> | (按位或) 对每一对比特位执行或（OR）操作

<details>
<summary>代码实例👈👈👈</summary>

```javascript
console.log(20.15|0);          // -> 20
console.log((-20.15)|0);       // -> -20
console.log(3000000000.15|0);  // -> -1294967296 🙁
```
</details>

---

### 3.黑科技number ^ 0版
+ 思路
>^ (按位异或)，对每一对比特位执行异或（XOR）操作。

<details>
<summary>代码实例👈👈👈</summary>

```javascript
console.log(20.15^0);          // -> 20
console.log((-20.15)^0);       // -> -20
console.log(3000000000.15^0);  // -> -1294967296 🙁
```
</details>

---

### 3.黑科技number << 0版
+ 思路
><< (左移) 操作符会将第一个操作数向左移动指定的位数。向左被移出的位被丢弃，右侧用 0 补充。

<details>
<summary>代码实例👈👈👈</summary>

```javascript
console.log(20.15 < < 0);     // -> 20
console.log((-20.15) < < 0);  //-20
console.log(3000000000.15 << 0);  // -> -1294967296 🙁
```
</details>

---

### 黑科技版总结：

>按位运算符方法执行很快，当你执行数百万这样的操作非常适用，速度明显优于其他方法。但是代码的可读性比较差。还有一个特别要注意的地方，处理比较大的数字时（当数字范围超出 ±2^31−1 即：2147483647），会有一些异常情况。使用的时候明确的检查输入数值的范围。


## 📚数组求和

[⬆ Back to top](#第一卷目录)

### 1.普通版
+ 思路
>遍历 相加

+ 优缺点比较

|优点|通俗易懂，简单粗暴|
|--|--|
|缺点|没有亮点，太通俗|

<details>
<summary>代码实例👈👈👈</summary>

```javascript
let arr = [1, 2, 3, 4, 5]
function sum(arr){
    let x = 0
    for(let i = 0; i < arr.length; i++){
        x += arr[i]
    }
    return x
}
sum(arr) // 15
```
</details>

---

### 2.优雅版
+ 思路
>return arr.reduce((a, b) => a + b)

+ 优缺点比较

|优点|简单明了，数组迭代器方式清晰直观|
|--|--|
|缺点|不兼容 IE 9以下浏览器|

<details>
<summary>代码实例👈👈👈</summary>

```javascript
let arr = [1, 2, 3, 4, 5]
function sum(arr) {
    return arr.reduce((a, b) => a + b)
}
sum(arr) //15
```
</details>

---

### 3.终极版
+ 思路
>return arr.reduce((a, b) => a + b)

+ 优缺点比较

|优点|让人一时看不懂的就是"好方法"。|
|--|--|
|缺点|1. eval 不容易调试。用 chromeDev 等调试工具无法打断点调试，所以麻烦的东西也是不推荐使用的…<br/><br/> 2. 性能问题，在旧的浏览器中如果你使用了eval，性能会下降10倍。在现代浏览器中有两种编译模式：fast path和slow path。fast path是编译那些稳定和可预测（stable and predictable）的代码。而明显的，eval 不可预测，所以将会使用 slow path ，所以会慢。|

<details>
<summary>代码实例👈👈👈</summary>

```javascript
let arr = [1, 2, 3, 4, 5]
function sum(arr) {
    return eval(arr.join("+"))
}
sum(arr) //15
```
</details>

---

## 📚单行写评级组件

[⬆ Back to top](#第一卷目录)

原文链接：[来自知乎用户蜗牛老湿的回答](https://www.zhihu.com/question/46943112/answer/113583615)
### 1.奇淫技巧之普通版

<details>
<summary>代码实例👈👈👈</summary>

```javascript
"★★★★★☆☆☆☆☆".slice(5 - rate, 10 - rate);
//定义一个变量rate是1到5的值，然后执行上面代码
//rate是1即一星，2是二星
```
</details>

---

### 1.奇淫技巧之进阶版
支持小数 
+ 思路
>先用☆☆☆☆☆当背景，然后把★★★★★放在上层，通过控制width+overflow就可以轻松支持小数字，不仅仅是2.5, 3.8也支持 毕竟我们宽度用em单位

<details>
<summary>代码实例👈👈👈</summary>
//html

```html
<div>☆☆☆☆☆</div>
```
//css
```html
div {
  position:relative;
}
div::after{
  content:'★★★★★';
  position:absolute;
  top:0;
  left:0;
  width:2.5em;
  overflow: hidden;
}
```
</details>

---
## 📚优雅的取随机字符串

[⬆ Back to top](#第一卷目录)

<details>
<summary>代码实例👈👈👈</summary>

```javascript
Math.random().toString(16).substring(2) 
Math.random().toString(36).substring(2) 
```
</details>

---

## 📚深拷贝

[⬆ Back to top](#第一卷目录)

不考虑IE的情况下，标准JSON格式的对象蛮实用，不过对于undefined和function的会忽略掉。

<details>
<summary>代码实例👈👈👈</summary>

```javascript
var a = {
    a: 1,
    b: { c: 1, d: 2 }
}
var b=JSON.parse(JSON.stringify(a))
```
</details>

---


## 📚1字符串转换成数字

[⬆ Back to top](#第一卷目录)

强大的隐式转换
<details>
<summary>代码实例👈👈👈</summary>

```javascript
var a = '1' 
+a
```
</details>

---


## 📚数组中最大值和最小值

[⬆ Back to top](#第一卷目录)

<details>
<summary>代码实例👈👈👈</summary>

```javascript
var numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411]; 
var maxInNumbers = Math.max.apply(Math, numbers); //122205
var minInNumbers = Math.min.apply(Math, numbers);//-85411
```
</details>

---


## 📚数组查找

[⬆ Back to top](#第一卷目录)

<details>
<summary>正常版本👈👈👈</summary>

```javascript
function find (x, y) {
  for ( let i = 0; i < x.length; i++ ) {
    if ( x[i] == y ) return i;
  }
  return null;
}
 
let arr = [0,1,2,3,4,5]
console.log(find(arr, 2))
console.log(find(arr, 8))
```
</details>

<details>
<summary>函数式版本👈👈👈</summary>

```javascript
const find = ( f => f(f) ) ( 
        f =>(next => (x, y, i = 0) =>
            ( i >= x.length) ? null : ( x[i] == y ) ? i :next(x, y, i+1)
        )(
            (...args) =>(f(f))(...args)
        )
    )
 
let arr = [0,1,2,3,4,5]
console.log(find(arr, 2))
console.log(find(arr, 8))
```
</details>

---
