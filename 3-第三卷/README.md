# 说明
> 主要参考链接如下（当然也有部分是自己归纳的知识点）

+ [编写自己的代码库（javascript常用实例的实现与封装）](https://segmentfault.com/a/1190000010225928)
+ [打造自己的JavaScript武器库](https://segmentfault.com/a/1190000011966867)
+ [为什么我认为数据结构与算法对前端开发很重要？](https://github.com/LeuisKen/leuisken.github.io/issues/2)
+ [some js plugs](https://github.com/Stevenzwzhai/plugs)


# 第三卷目录

* [获取元素最终背景色](#📚获取元素最终背景色)
* [命名方式转换](#📚命名方式转换)
* [扁平化数组转换为树状结构](#📚扁平化数组转换为树状结构)
* [Cookie]()
* [判断`obj`是否为空]()
* [Random]()
* [现金额转大写]()
* [函数节流]()
* [函数防抖]()
* [随机码toString详解]()
* [计算指定字符出现次数]()

# 实例


## 📚获取元素最终背景色

[⬆ Back to top](#第三卷目录)

>背景：用JS代码求出页面上一个元素的最终的background-color，不考虑IE浏览器，不考虑元素float情况

+ 思路

|内敛样式|内联样式可以通过元素的style属性获取，如果style属性有background-color值，则可以直接获取出来 (暂不考虑!important) |
|--|--|
|外联的层叠样式|DOM2样式规范在document.defaultView中包含了一个getComputedStyle()方法。该方法返回一个只读的CSSStyleDeclaration对象，其中包含特定元素的所有计算样式。|


<details>
<summary>代码实例👈👈👈</summary>

```javascript
//字符串转换为驼峰
//将连字符类的css属性值，转换成驼峰写法
//将background-color转换为backgroundColor
function camelize(str){
    return str.replace(/-(\w)/g, function(strMatch, newStr){
        return newStr.toUpperCase();
    })
}

//获取计算后的样式
function getStyle(ele, property) {
    //安全保护性的判断
    if (!ele || !property) {
        return;
    }
    var defaultValue = ele.style[camelize(property)];
    var css = null;
    //功能嗅探
    if (!defaultValue) {
        if (document.defaultView && document.defaultView.getComputedStyle) {
            css = document.defaultView.getComputedStyle(ele, null);
            defaultValue = css ? css.getPropertyValue(property) : null;
        }

    }
    return defaultValue;
}

//排除特殊情况
//检查获取背景色的有效性
function checkBgColor(ele){
    var defaultValue = getStyle(ele, "background-color");
    var hasColor = defaultValue ? true : false;//是否有颜色
    if(value == "transparent" || value == "rgba(0,0,0,0)"){
        //未设置background-color，或者设置为跟随父节点
        hasColor = false;
    }else if(getStyle(ele, "display") == "none" || getStyle(ele, "visivility" == "hidden")){
        // dom节点不可见
        hasColor = false;
    }else if(getStyle(ele, "opacity") == "0"){
        // dom节点透明度为全透明
        hasColor = false;
    }
    return hasColor;
}

//检测父节点
//这里检测父节点主要是针对父节点设置了隐藏属性(display:none;visibility:hidden;)
function checkParent(ele){
    var parent = ele.parentNode;
    var hasColor = true;
    //一般来说是不会把body设为隐藏
    if(parent.nodeName == "BODY"){
        return hasColor;
    }
    if(getStyle(parent, "display") == "none" || getStyle(parent, "visivility" == "hidden")){
        hasColor = false;
        return hasColor;
    }else{
        checkParent(parent);
    }
}
//获取最终颜色
function getRealBg(ele){
    //如果父元素为隐藏，就不用再获取元素
    if(!checkParent(ele)){
        return ''
    }
    if(checkBgValue(ele)){
        return getStyle(ele, 'background-color');
    }
    //如果已经回溯到html根节点，则可以停止回溯。
    else if(ele != document.documentElement){
        return getRealBg(ele.parentNode);
    }
    return '';
}
```
</details>

P.S.当然还有新思路：**canvas截图+Js获取图片某点颜色，这样可以完美解决所有的问题。**

### 参考链接：
+ [获取元素的最终background-color](https://www.jianshu.com/p/e94b5779f998)
+ [一月前端面试--获取元素最终背景色](https://www.jianshu.com/p/e09c67c3bd98)

---

## 📚命名方式转换

[⬆ Back to top](#第三卷目录)

> 连字符转驼峰 & 驼峰转连字符

<details>
<summary>连字符转驼峰👈👈👈</summary>

```javascript
// 连字符转驼峰
String.prototype.hyphenToHump = function () {
  return this.replace(/-(\w)/g, (...args) => {
    return args[1].toUpperCase()
  })
}
```
</details>

<details>
<summary>驼峰转连字符👈👈👈</summary>

```javascript
// 驼峰转连字符
String.prototype.humpToHyphen = function () {
  return this.replace(/([A-Z])/g, '-$1').toLowerCase()
}
```
</details>

---

## 📚扁平化数组转换为树状结构

[⬆ Back to top](#第三卷目录)

> 常用于后台发来的扁平化数组结构的数据转换为树状结构便于生成前端菜单

<details>
<summary>代码实例👈👈👈</summary>

```javascript

const arrayToTree = (arr,id='id',pid='pid',children='children') =>{
    let data =  JSON.parse(JSON.stringify(arr));
    let result = []
    let hash = {}
    data.forEach((item, index) => {
        hash[data[index][id]] = data[index] //生成一个以为id为数组下标的数组 【多个数组变为了一个
    })
    data.forEach((item) => {
        item.key = item.id
        let hashVP = hash[item[pid]]
        if (hashVP) {//如果有父级  则将元素放在该父级的孩子节点上
            !hashVP[children] && (hashVP[children] = [])
            hashVP[children].push(item)
        }
        else if(!(hashVP&&item.route)){//第一级：1.没有父亲节点 且没有路由属性
          result.push(item)
        }
    })
    return result;
}

```
</details>

---