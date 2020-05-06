---
title: ES6模块的用法和坑
date: 2019-01-01 17:48:30
tags:
---

> ES6给JavaScript带来了原生的模块化，使得我们的开发更加便利。ES6的模块化包含有导出(export)与导入(import)两种方法

## export的用法
在大家比较熟悉的commonJS模块化规范中，每个文件即是一个模块。ES6中也同样引入了这样的概念。一个文件即是一个模块，在模块中的定义的变量、函数等在外部是无法直接访问到的。需要通过使用export来对其进行暴露，然后其他文件通过import引入才能访问。比如以下这个例子。我们先创建一个test1.js的模块，里面有一个方法

```js
# test1.js
let name = "Lili";

function getName(){
    return name;
}

export default getName;
```