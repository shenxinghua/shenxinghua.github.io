---
title: JS数组去重常见方法分析
date: 2018-11-10
datestring: "20181110"
pageid: 1
categories: 
- 算法
tags:
- 算法
- js
photos: /20181110/1/724.jpg
summary: 数组去重是开发中经常会遇到的问题，也是面试时经常会考到的。JS实现数组去重可以有多种方法，本文将会详细分析。
---


数组去重是开发中经常会遇到的问题，也是面试时经常会考到的。JS实现数组去重可以有多种方法：

## 一、简单的去重方法

用一个类比来简单解释一下这种去重方法的思路：A篮子里有若干个不同颜色和大小的球，旁边放一个空篮子B。 我们将球挨个从A篮子里拿出来，如果B篮子里没有和手上拿着一模一样的球，就把它放到篮子B里。如果B篮子里已经有了一模一样的球，就把手上的球丢掉。那等到A篮子里被取空的时候，B篮子的球就会彼此之间都不完全相同。代码如下：
 
 ```js
// 简单数组去重法

function unique(arr){
    var temp = []; 
    for(var i = 0; i < arr.length; i++){
        if(temp.indexOf(arr[i]) == -1){
            temp.push(arr[i]);
        }
    }
    return temp;
}

var arr1 = [1,2,2,3,5,3,5,9];

console.log(unique(arr1)); 

var arr2 = ['a','c','e','a','c','f'];

console.log(unique(arr2));

var arr3 = [1,2,"2",2,'a','c',3,'2','e',5,3,5,'a',9,'c','f'];

console.log(unique(arr3)); 

var arr4 = [1,2,"2",2,'a','c',3,'2',{k:123},'e',5,3,5,'a',9,[1,2,3],'c','f',[1,2,3],[1,'2',3],[4,5,6],{k:123},{k:'123'},{f:234}];

console.log(unique(arr4)); 

```
得到的结果为：

```shell
[1, 2, 3, 5, 9]

["a", "c", "e", "f"]

[1, 2, "2", "a", "c", 3, "e", 5, 9, "f"]

[1, 2, "2", "a", "c" 3, {k:123}, "e", 5, 9, [1,2,3], "f", [1,2,3], [1,"2",3], [4,5,6], {k:123}, {k:"123"}, {f:234} ]
```


需要说明的是**由于数组和对象都是引用类型，对比时实际上是在对比指针地址，所以是不会相等的，以下的其他方法也遵循该原则**。

此方法从效果来讲，还是符合要求的。

当然有人会说 IE8等旧浏览器不支持array的indexOf的方法，那我们也很容易用遍历的方法来代替indexOf方法。但我觉得大多数情况下，为了爱惜生命，我们早就应该远离IE8等古董级浏览器了。

## 二、排序相邻去重法

解题思路为： 将数组的所有元素按统一标准排序。那么相同的元素肯定会相邻。然后去除重复的元素。代码如下：

```js
//排序相邻去重法

function unique(arr){
    arr.sort();
    var temp=[arr[0]];
    for(var i = 1; i < arr.length; i++){
        if( arr[i] !== temp[temp.length-1]){
            temp.push(arr[i]);
        }
    }
    return temp;
}

var arr1 = [1,2,2,3,5,3,5,9];

console.log(unique(arr1)); 

var arr2 = ['a','c','e','a','c','f'];

console.log(unique(arr2));

var arr3 = [1,2,"2",2,'a','c',3,'2','e',5,3,5,'a',9,'c','f'];

console.log(unique(arr3)); 

var arr4 = [1,2,"2",2,'a','c',3,'2',{k:123},'e',5,3,5,'a',9,[1,2,3],'c','f',[1,2,3],[1,'2',3],[4,5,6],{k:123},{k:'123'},{f:234}];

console.log(unique(arr4)); 

```

结果为：

```
[1, 2, 3, 5, 9]

["a", "c", "e", "f"]

[1, 2, "2", 2, "2", 3, 5, 9, "a", "c", "e", "f"]

[1, [1, 2, 3], [1, 2, 3], [1, "2", 3], 2, "2", 2, "2", 3, [4, 5, 6], 5, 9, {k: 123}, {k: 123}, {k: "123"}, {f: 234}, "a", "c", "e", "f"]
```

从结果中看出，此方法只在纯数字或者纯字符的数组中有效。究其原因为排序出了问题。 

解决的思路为：我们可以将数组中的元素按类型分到几个不同的数组中，然后分别各自排序相邻去重，然后再合并为一个数组，但这样显然是比较复杂了。


## 三、数组下标法

实现思路：遍历数组中的每一项，如果当前数组的第i项在当前数组中第一次出现的位置不是i，那说明i项的值存在重复。 这个方法比上面的两种方法更简单有效。 代码如下：

```js
//数组下标法

function unique(arr){
    var temp = [];
    for(var i = 0; i < arr.length; i++) {
        if(arr.indexOf(arr[i]) == i){
            temp.push(arr[i])
        }
    }
    return temp;
}

var arr1 = [1,2,2,3,5,3,5,9];

console.log(unique(arr1)); 

var arr2 = ['a','c','e','a','c','f'];

console.log(unique(arr2));

var arr3 = [1,2,"2",2,'a','c',3,'2','e',5,3,5,'a',9,'c','f'];

console.log(unique(arr3)); 

var arr4 = [1,2,"2",2,'a','c',3,'2',{k:123},'e',5,3,5,'a',9,[1,2,3],'c','f',[1,2,3],[1,'2',3],[4,5,6],{k:123},{k:'123'},{f:234}];

console.log(unique(arr4)); 
```

结果为：

```shell
[1, 2, 3, 5, 9]

["a", "c", "e", "f"]

[1, 2, "2", "a", "c", 3, "e", 5, 9, "f"]

[1, 2, "2", "a", "c", 3, {k: 123}, "e", 5, 9, [1, 2, 3], "f", [1, 2, 3], [1, "2", 3], [4, 5, 6], {k: 123}, {k: "123"}, {f: 234}]
```

此方法从效果来讲，是符合要求的。


## 四、对象键值去重法

解题思路：新建一个对象以及新数组，遍历传入数组时，判断值是否为对象的键，不是的话给对象新增该键并放入新数组。 不同的键可能会被误认为一样，例如n[1]、n["1"]，所以还得加上类型的判断。 

```js
//对象键值去重法

function unique(arr){
    var temp = {}, result = [], len = arr.length, val, type;
    for (var i = 0; i < len; i++) {
        val = arr[i];
        type = typeof val;
        if (!temp[val]) {
            temp[val] = [type];
            result.push(val);
        } else if (temp[val].indexOf(type) < 0) {
            temp[val].push(type);
            result.push(val);
        }
    }
    return result;
}


var arr1 = [1,2,2,3,5,3,5,9];

console.log(unique(arr1)); 

var arr2 = ['a','c','e','a','c','f'];

console.log(unique(arr2));

var arr3 = [1,2,"2",2,'a','c',3,'2','e',5,3,5,'a',9,'c','f'];

console.log(unique(arr3)); 

var arr4 = [1,2,"2",2,'a','c',3,'2',{k:123},'e',5,3,5,'a',9,[1,2,3],'c','f',[1,2,3],[1,'2',3],[4,5,6],{k:123},{k:'123'},{f:234}];

console.log(unique(arr4)); 
```

结果为：

```js
[1, 2, 3, 5, 9]

["a", "c", "e", "f"]

[1, 2, "2", "a", "c", 3, "e", 5, 9, "f"]

[1, 2, "2", "a", "c", 3, {k: 123}, "e", 5, 9, [1, 2, 3], "f", [4, 5, 6]]
```

从结果中可以发现这种方法的缺点是：当数组中的元素为对象时，当其作为键时会自动执行该对象的toString方法，也就是说对象一般会转换成键值“[object Object]”。 而数组中的元素为数组时，只要toString()后的值相同就会被认为是同一个值。 

解决的方法为：在遇到数组和对象时，进行特殊处理。修正后的代码如下：

```js
function unique(arr){
    var temp = {}, result = [], len = arr.length, val, type;
    for (var i = 0; i < len; i++) {
        val = arr[i];
        type = typeof val;
		
		if(type == "object"){
			result.push(val);
		}else if (!temp[val]) {
            temp[val] = [type];
            result.push(val);
        } else if (temp[val].indexOf(type) < 0) {
            temp[val].push(type);
            result.push(val);
        }
    }
    return result;
}
```

修正后就可以得到想要的结果了。 (〃'▽'〃)

## 五、优化遍历数组法

方法的思路为： 从数组第0项开始，每次取一个值，把它与其后面的所有元素进行对比，只取后面没有与它重复的值。

```js
//优化遍历数组法

function unique(arr){
    var temp = [];
    var index = [];
    var l = arr.length;
    for(var i = 0; i < l; i++) {
        for(var j = i + 1; j < l; j++){
            if (arr[i] === arr[j]){
                i++;
                j = i;
            }
        }
        temp.push(arr[i]);
        index.push(i);
    }

    return temp;
}

var arr1 = [1,2,2,3,5,3,5,9];

console.log(unique(arr1)); 

var arr2 = ['a','c','e','a','c','f'];

console.log(unique(arr2));

var arr3 = [1,2,"2",2,'a','c',3,'2','e',5,3,5,'a',9,'c','f'];

console.log(unique(arr3)); 

var arr4 = [1,2,"2",2,'a','c',3,'2',{k:123},'e',5,3,5,'a',9,[1,2,3],'c','f',[1,2,3],[1,'2',3],[4,5,6],{k:123},{k:'123'},{f:234}];

console.log(unique(arr4));
```

得到的结果为：

```shell
[1, 2, 3, 5, 9]

["e", "a", "c", "f"]

[1, 2, "2", "e", 3, 5, "a", 9, "c", "f"]

[1, 2, "2", {k:123}, "e", 3, 5, "a", 9, [1,2,3], "c", "f", [1,2,3], [1,"2",3], [4,5,6], {k:123}, {k:"123"}, {f:234} ]
```

从效果来看，此方法是符合要求的。

## 六、 Set去重法

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。 其用于数组去重的代码如下所示：


```js
//Set去重法

const unique = arr => [...new Set(arr)]; 


var arr1 = [1,2,2,3,5,3,5,9];

console.log(unique(arr1)); 

var arr2 = ['a','c','e','a','c','f'];

console.log(unique(arr2));

var arr3 = [1,2,"2",2,'a','c',3,'2','e',5,3,5,'a',9,'c','f'];

console.log(unique(arr3)); 

var arr4 = [1,2,"2",2,'a','c',3,'2',{k:123},'e',5,3,5,'a',9,[1,2,3],'c','f',[1,2,3],[1,'2',3],[4,5,6],{k:123},{k:'123'},{f:234}];

console.log(unique(arr4));

```

结果为：

```shell
[1, 2, 3, 5, 9]

["a", "c", "e", "f"]

[1, 2, "2", "a", "c", 3, "e", 5, 9, "f"]

[1, 2, "2", "a", "c", 3, {k: 123}, "e", 5, 9, [1, 2, 3], "f", [1, 2, 3], [1, "2", 3], [4, 5, 6], {k: 123}, {k: "123"}, {f: 234}]
```

好棒！ 强大又简洁！ 唯一的缺点是IE11以下的版本都不支持。 (ಥ﹏ಥ)


综上所述，简单的去重方法、数组下标法、优化遍历数组法和Set去重法适用性比较好。 排序相邻去重法和对象键值去重法容易出现问题。大家可灵活运用。

***不知不觉就很晚了，该休息了~~ 解决方法有很多，欢迎大家补充！***