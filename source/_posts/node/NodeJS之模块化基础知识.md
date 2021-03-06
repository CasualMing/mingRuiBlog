---
title: NodeJS模块化基础知识
date: 2017-12-26
categories: Node
tags:
  - NodeJs
  - 模块化区分及历史发展
---

> 主要记录一下各种模块化规范及怎么使用模块化开发

<!-- more -->

## 前言

**认识模块化之前，要了解，为啥要使用模块化，使用模块化开发，有什么好处，该怎么使用模块化开发。**

> 模块化开发，将重复的代码提取到公共文件内。然后当需要这段代码的时候，就可以直接引入到需要的地方，这样可以大大节省代码量，可以大大节省开发时间。并且，当代码出了错误的时候，可以根据对应的错误找到对应的代码进行审查，不用审查全部代码。这样可以更方便维护。


## 历史

> 加载模块化，要遵循 `CommonJS` 规范，在浏览器端，不能实现模块化，因为加载模块是同步的，这个时候就出来了两种加载模式 `AMD` `CMD` ,这两种方式加载模块，都是异步的。

#### 1、`CommonJS` 是什么?

其实就是一套约定俗成的约定。不过这个约定是约定了。在JavaScript中。各个模块是如何相互依赖的，同时规定了模块的特性，并且，是实现同步加载模块的。

#### 2、AMD规范

- 规定了模块之间的依赖关系。它推崇提前说明依赖，所以当你要弄一个模块的时候，需要提前想好各模块之间的依赖关系。不然当需要添加依赖的时候，就需要重新回到开头添加依赖。
- ** `AMD` 是和 `CommentJS` 兼容的,并且 `AMD` 是异步的。 **
- `AMD` 可以作为 `CommonJS` 模块一个中转的版本，只要 `CommonJS` 没有被用来同步的 `require` 调用。使用同步 `require` 调用的 `CommonJS` 代码可以被转换为使用回调风格的AMD模块加载器

#### 3、CMD规范

 - 它同样是规定了模块之间的依赖关系。但是不同 `AMD` 的提前说明。它更推崇在使用过程中声明依赖关系。 也就是不管你代码也到哪儿了。只要你觉得这个地方需要添加新的模块依赖项，那么就可以直接 `require` 加载依赖项，`CMD`规范会直接帮你搞定预加载
 
- 与 `AMD` 的预加载以及 预加载依赖这两项上是一样的。都是使用`require` 和 `difine`
- 它推崇就近原则。

### 4、ES6模块化(使用主流)


  - **ES6模块化特点**
   - 每一个模块只加载一次， 每一个JS只执行一次， 如果下次再去加载同目录下同文件，直接从内存中读取。 一个模块就是一个单例，或者说就是一个对象；
   - 每一个模块内声明的变量都是局部变量， 不会污染全局作用域；
   - 模块内部的变量或者函数可以通过export导出；
   - 一个模块可以导入别的模块


## Node作用域

 -  在 nodeJS 中，只有两类作用域。一类是模块作用域，一类是全部作用域。对应着，也就产生了模块变量 `module` ，全局变量 `global`。
 - 每一个js文件其实就是一个独立的模块。默认是无法被外界访问的

#### 全局变量global

当模块之间想传递某些属性或者方法的时候，可以将其挂载到全局变量 `global` 树上，这个就相当于JavaScript中将变量挂载到 `window` 树上。这种方法一般不推荐使用，因为这样容易引起全局变量污染。

#### 模块作用域

 - **模块标识符 `module`**

 
`module` 属性是 `CommonJS` 中的一个对象，可以表示当前具体的模块
 

 - **暴露模块成员 `exports`**


 由于模块之间，默认是无法相互访问的，如果想这个对象被别人访问，那么就需要通过 `exports` 对象，暴露给外界。其实就是将方法挂载到了`exports` 树上


 - **暴露模块成员 `module.exports`**

 这个方法其实跟上边的方法用法一样

 
- **`exports` 与 `module.exports`的异同之处**

  - **不同：** 当同一模块 `exports` 及 `module.exports` 都挂在了一个相同的属性的时候，那么就以 `module.exports`为准。
  
  - **相同：** 都有将属性暴露的作用。
  
  - **相同：** 都引用了同一块对象地址
 
 - **引入模块 `require` **
遵循CommonJS规范的模块，都有一个 `require` 对象。这个对象可以将模块中暴露出来的成员，很轻松的引入到当前需要的文件内。


#### NodeJS中的模块概念。


在Node中，模块分三种

**一：** 是核心模块，这种一般是安装NodeJS环境的时候，就会自带

**二：** 是第三方模块，这种一般是在网上下载使用的 NodeJS模块，可以根据对应的文档，作出使用。

**三：** 是用户自定义模块。这种一般为用户在使用模块的时候，根据业务需求，作出一些适合自己当前情况的模块。（用户自己在JavaScript文件中写的代码也可以称之为用户自定义模块）


### 用户自定义的模块如何使用


 - 模块1:

  ```javascript
    var a= 100 ;
    function say() {
      console.log('okokok')
    }
  module.exports={a}; //ES6中语法，如果key跟value相同，那么就可以只写一项。
  ```


 - 模块2：
 

 ```javascript
 var b = 200 ;
 module.exports.b = b;
 ```


此时如果第三方文件需要变量 a 和变量 b 的话，那么就需要将 a 和 b 暴露出去。

 
 - 使用模块的文件：
 

  ```javascript

   const m1 = require('./03.模块1.js');

   const m2 = require('./04.模块2.js');

   console.log(m1.a*m2.b); // 输出结果： 100*200=20000 
 ```


如果要像 `CommonJS` 中不用引入文件路径，只用引入文件名称就可以使用的话，那么就需要新建一个名称为 `node_modules` 的文件夹。然后将模块文件放到 `node_modules` 的文件内。需要什么模块时，可以直接引入对应的文件名就可以了。


 **注意：要使用什么模块里的方法，第一步就需要将什么模块用 `require` 引入，否则就会报错，并且引入的名称需要跟 `node_modules`中对应的文件名要一致 **