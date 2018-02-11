# 前端各类Polyfill实现
>简单来讲，写些代码判断当前浏览器有没有这个功能，没有的话，就写一些支持的补丁代码(polyfill)。写polyfill让我们更接近原生，考验编码能力

## 参考链接
+ [https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills)


## 目录
+ indexOf /lastIndexOf 
+ bind
+ debounce
+ once
+ 深浅拷贝


## 📚 indexOf /lastIndexOf

[⬆ Back to top](#目录)

### 1.
+ 思路
>首先如果分开来写，即两个方法相对独立地写，很显然代码量会比较多，因为两个方法功能相似，所以可以想办法调用一个方法，将不同的部分当做参数传入，减少代码量。其次，如果数组已经有序，是否可以用更快速的二分查找算法？这点会是加分项。

<details>
<summary>代码实例👈👈👈</summary>

```javascript

```
</details>

---


## 📚 bind

[⬆ Back to top](#目录)

>bind是function的一个函数扩展方法，bind以后代码重新绑定了func内部的this指向（obj），但是不兼容ie6~8

**参考链接**
+ [关于 bind 你可能需要了解的知识点以及使用场景](https://github.com/hanzichi/underscore-analysis/issues/18)
+ [bind 方法的兼容实现](https://github.com/hanzichi/underscore-analysis/issues/19)

<details>
<summary>代码实例👈👈👈</summary>

```javascript

```
</details>

---