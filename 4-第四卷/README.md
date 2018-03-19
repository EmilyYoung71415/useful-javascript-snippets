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
//数组的indexOf方法封装
function indexOf(arr,value,start){
    //如果不设置start,则默认start为0
    if(arguments.length == 2){
        start = 0;
    }
    //如果数组中存在indexOf方法，则用原生的indexOf方法
    if(arr.indexOf){
        return arr.indexOf(value,start);
    }
    for(var i = start; i < arr.length; i++){
        if(arr[i] === value){
            return i;
        }
    }
    return -1;
}
```
</details>

---


## 📚 bind

[⬆ Back to top](#目录)

>bind是function的一个函数扩展方法，bind以后代码重新绑定了func内部的this指向（obj），但是不兼容ie6~8

>**bind() 方法会创建一个新函数**，当这个新函数被调用时，**它的 this 值是传递给 bind() 的第一个参数**, 它的参数是 bind() 的其他参数和其原本的参数。

**参考链接**
+ [关于 bind 你可能需要了解的知识点以及使用场景](https://github.com/hanzichi/underscore-analysis/issues/18)
+ [bind 方法的兼容实现](https://github.com/hanzichi/underscore-analysis/issues/19)
+ [Javascript之bind ](https://github.com/Aaaaaaaty/Blog/issues/1)
<details>
<summary>代码实例👈👈👈</summary>

```javascript
//简化版
Function.prototype.bind = function( context ){ //【0】 context即我们预绑定的对象
    var self = this; // 【1】 本：闭包this 指向全局
    return function(){ 
        return self.apply( context, arguments ); 
        //self.apply()是真正的执行，恢复正常的this指针，并将函数参数作为参数传递给调用函数
    }
};

Function.prototype.bind = function(){
    var self = this, // 保存原函数
    context = [].shift.call( arguments ), // 需要绑定的 this 上下文
    args = [].slice.call( arguments ); // 剩余的参数转成数组
    return function(){ // 返回一个新的函数
        return self.apply( 
            context, [].concat.call( args, [].slice.call( arguments ) ) 
        );
    // 执行新的函数的时候，会把之前传入的 context 当作新函数体内的 this
    // 并且组合两次分别传入的参数，作为新函数的参数
    }
};
var 
```
</details>

---

## 📚getElementsByClassName

[⬆ Back to top](#目录)
> 如果参数类名列表由空格分隔，则进行且匹配，即只有元素中的类名包含参数类名列表中的所有类名才算匹配成功；如果参数类名列表由逗号分隔，则进行或匹配，即只要元素中的类名包含参数类名列表中的其中一个类型就算匹配成功


<details>
<summary>代码实例👈👈👈</summary>

```javascript
function getElementsByClassName(parentObj,classStr){
    var result = [];
    //获取parentObj下的所有子元素
    var objs = parentObj.getElementsByTagName('*');
    //条件一：如果classStr用空格分隔，则表示class必须同时满足才有效
    var targetArr1 = classStr.trim().split(/\s+/).noRepeat();
    //条件二：如果classStr用逗号分隔，则表示class只要有一个满足就有效
    var targetArr2 = classStr.trim().split(/\s*,\s*/).noRepeat();
    //只有一个class或者进行条件一测试
    if(classStr.indexOf(',') == -1 ){
        label: for(var i = 0; i < objs.length; i++){
            //获取每一个子元素的类名，将其转换为数组后去重
            var arr = objs[i].className.trim().split(/\s+/).noRepeat();
            //进入循环，测试是否符合条件一
            for(var j = 0; j < targetArr1.length; j++){
                //如果条件一中的某一项在arr数组中不存在，则跳过该子元素
                if(!arr.inArray(targetArr1[j])){
                    continue label;
                }
            }
            //将符合条件一的子元素对象放在结果数组中
            result.push(objs[i]);
        }
        //返回结果数组
        return result;
    //进行条件二测试
    }else{
        label: for(var i = 0; i < objs.length; i++){
                //获取每一个子元素的类名，将其转换为数组后去重
                var arr =objs[i].className.trim().split(/\s+/).noRepeat();
                //进入循环，测试是否符合条件二
                for(var j = 0; j < targetArr2.length; j++){
                    //只要条件二的中某一项在arr数组中存在，就符合
                    if(arr.inArray(targetArr2[j])){
                        //将符合条件二的子元素对象放在结果数组中
                        result.push(objs[i]);
                        //接着进入下一个子元素测试
                        continue label;
                    }
                }   
            }
        //返回结果数组
        return result;     
    }
}
```
</details>

---

