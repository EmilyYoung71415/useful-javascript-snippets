# 你以为很简单的问题
>一些问题看似看简单，但是实际上你[第一反应]的往往都是特别辣鸡的code。有些或许你兼容性没考虑好，或许异常没考虑，亦或是压根就南辕北辙了..

# 目录
+ 类型判断



## 📚 类型判断

[⬆ Back to top](#目录)

### 1. 判断元素是否是数组

**参考链接**

[Javascript中判断数组的正确姿势](http://www.cnblogs.com/zichi/p/5103842.html)
+ 思路
>ES5 提供的 Array.isArray() 方法,但兼容不够。数组的 typeof 结果也是 object，而因为 null 的结果也是object. 进阶：**Object.prototype.toString()**

<details>
<summary>代码实例👈👈👈</summary>

```javascript
function isArray(a) {
  Array.isArray ? Array.isArray(a) : Object.prototype.toString.call(a) === '[object Array]';
}
```
</details>

---

### 2. 判断元素是否是对象

<details>
<summary>代码实例👈👈👈</summary>

```javascript
// Is a given variable an object?
// 判断是否为对象
// 这里的对象包括 function 和 object
_.isObject = function(obj) {
  var type = typeof obj;
  return type === 'function' || type === 'object' && !!obj;
};
```
</details>

---

### 3. 判断'Arguments', 'Function', 'String', 'Number', 'Date', 'RegExp', 'Error' 
+ 思路
>都可以用 Object.prototype.toString.call 来判断

<details>
<summary>代码实例👈👈👈</summary>

```javascript
// 其他类型判断
_.each(['Arguments', 'Function', 'String', 'Number', 'Date', 'RegExp', 'Error'], function(name) {
  _['is' + name] = function(obj) {
    return toString.call(obj) === '[object ' + name + ']';
  };
});
```
</details>

---


### 4. 判断元素是否是DOM 元素

+ 思路
>保证它不为空，且 nodeType 属性为 1

<details>
<summary>代码实例👈👈👈</summary>

```javascript
// 判断是否为 DOM 元素
_.isElement = function(obj) {
  // 确保 obj 不是 null 
  // 并且 obj.nodeType === 1
  return !!(obj && obj.nodeType === 1);
};
```
</details>

---