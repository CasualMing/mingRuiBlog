---
title: NodeJS知识学习
date: 2017-11-13
categories: Node
tags:
  - NodeJs
  - NodeJs基础知识
---

> 主要记录一下学习Node的笔记

<!-- more -->

#NodeJS学习笔记


## 什么是node.js?


> node.js是基于谷歌开发的 `V8引擎` 以及 `JavaScript` ，并且以事件驱动和非阻塞I/O的可以在服务器端运行 JavaScript 代码的执行环境。

## node.js有啥用处


> 1. 可以实现对文件的读取与写入删除等管理。
> 2. 可以利用后端框架，开发桌面应用
例如： 
  基于 Socket 技术，开发类似于聊天室之类的**即时通讯项目**
  使用 Node.js 开发一些**实用的工具** 或 **包**

## node.js与JavaScript的差别


- > JavaScript构成为：**ECMAscript** , **DOM** , **BOM**  

  > (其中ECMAscript规定了JavaScript的语言规范，DOM为文档对象模型，顶级对象为 `document` ,BOM为浏览器对象模型，顶级对象为 `Window` )

- >**node**的构成为：**ECMAscript核心** , **全局成员** , **node自带的核心API模块**
 
  >其中的全局成员为： `console` ,`setInterval` , `setTimeout` ...等
  
 > 核心API是node平台独有的。由于node的服务运行端中，没有浏览器跟HTML的概念，所以也就没有 `BOM` 跟 `DOM` 模型。

## 文件读写修改


 > 因为 nodeJS是基于模块化的，所以在使用什么功能之前，就需要在该文件内引入对应的模块。
 
- **1、文件内容读取 `fs.readFlie`**
 
 ```javascrit
   const fs = require("fs");  //语法：require("模块名")
   fs.readFile("./index,js","utf8",function(err,data){
      if(err) return console.log(err.message);
      console.log(data);
   })
  
 ``` 


其中使用 `fs` 模块中的 `readFile` 方法；内有三个参数，参数1：读取文件的路径；参数2：读取文件的编码；参数3：回调函数（回调函数内，有两个参数，参数1：错误函数；参数2：读取回来的文件内容），读取之后进行的操作

 - **2、文件内容写入 `fs.writeFile` **
 
> 用这个方法，虽然可以将内容写入到指定文件内，但是会将目标文件内原有的内容覆盖掉。所以这个方法不推荐使用


 ```javascript
    const fs = require("fs");
    fs.writeFile("./test.txt","你今天还好吗？","utf8",function(err){
         if(err) return console.log(err.message)
          console.log("写入成功")
    })
 ```


> 参数解读：写入文件方法，有四个参数:**参数1**：写入文件的路径；**参数2**：写入的内容，**参数3（默认:utf8）**：写入文件内的内容编码格式，**参数4**：回调函数（回调函数有一个参数：参数1：写入文件内容时是否发生错误。）

- **3、文件内容追加 `fs.appendFile` **


> 这个方法的参数跟上面的写入方法一样。
 
```javascript
    const fs = require("fs");
    fs.appendFile("./test.txt","你今天还好吗？","utf8",function(err){
         if(err) return console.log(err.message)
          console.log("写入成功")
    })
```


  - **4、读取文件信息 `fs.stat` **

> 这个方法能将指定文件内的信息
> 参数解读：参数1：读物目标的路径；参数2：读取的信息编码；参数3：读取信息之后的回调函数（回调函数有两个参数，参数1：读取的错误信息，参数2：读取的全部信息）。

```javascript
  const fs = require("fs");
  fs.stat("./tast.txt","utf8',function(error,data){
      console.log(error);
      console.log(data);
 })
```


 - **5、读取指定文件的文件名 fs.readdir** 

```javascript
  //写法1：
    const fs = require("fs");
    fs.readdir("./img",function(err,files){
    console.log(err);
    console.log(files)
   })

  //也可以这么写-----------
  //写法2：这种文件路径为绝对路径；
   const fs = require("fs");
   const path = require("path");
   fs.readdir(path.join(__dirname,"./img"),function(err,files){
    console.log(err);
    console.log(files)
   })
```


![代码跟要读取的文件路径](https://i.imgur.com/veATGqz.png)

> `fs.readdir` 这个函数中的参数1为要读取的文件URL路径，这个路径是相对于代码的


## 题目：


将同级目录下的 **img** 文件夹内的图片实行分类，将 **jpg** 图片跟 **png** 图片分别放在对应的文件夹内，并且对每个图片重新对其命名。

**思路：** 先用 `fs.readdir` 读取出 img 文件夹下每张图片的名字，然后用 ES6 中的 ` endsWith` 方法，区分是以 jpg 结尾的还是以 png 结尾的，然后分别放在对应的数组中，下一步循环数组，用 `fs.readFile`读取img文件夹内对应数组中的每一项图片信息，然后将每一项利用 `fs.appendFile` 将 img 文件夹内匹配的图片追加到已经建好的文件夹内。

 ```javascript
 const fs = require("fs");  
 const path = require("path"); //引入需要的模块、
 fs.readdir("./img","utf8",function(err,data){
    //console.log(data),data为一个元素为字符串的数组
   let jpg = [];
   let png = [];
   for(let i=0; i<data.length,i++){
      if(data[i].endsWith(".jpg")){
          //console.log("进来的说明是jpg图片")
          jpg.push(data[i]);
      }
      if(data[i].endsWith(".png")){
          //console.log("进来的说明是png图片")
          png.push(data[i]);
      }
   } //循环完之后就可以确定哪些是jpg,哪些是png
   //然后循环数组，去img图片文件夹内，找出对应的格式图片，将其写入到对应的文件夹。
  for(let j = 0;j<jpg.length;j++){ //此处循环，由于内部是异步的，所以内部使用jpg[j]的时候，j为jpg.length,这个时候根本找不到对应的jpg[j],
  //解决办法：1、for循环内定义j的时候用let,这样就可以产生一个局部作用域，2、使用一个自调用函数，将j传到函数内，也是产生一个局部作用域
    fs.readFile("./img/"+jpg[j],function(err,data){
          //console.log(jpg[j])
          if(err) return console.log(err.message);
          //console.log(data);
       fs.appendFile("./jpg/"+"crm-"+j+"."+jpg[j].split(".")[1],data,function(err){
        console.log(err);
       })
    })
  }
 for(let t = 0;t<png.length;t++){
    fs.readFile("./img/"+png[t],function(err,data){
          if(err) return console.log(err.message);
          console.log(data);
       fs.appendFile("./jpg/"+"crm-"+t+"."+jpg[t].split(".")[1],data,function(err){
        console.log(err);
       })
    })
  }
})
 ```


上面的写法可以再次进行精简

```javascript
   const fs = require("fs");  
   const path = require("path"); //引入需要的模块、
 fs.readdir("./img","utf8",function(err,data){
    //console.log(data),data为一个元素为字符串的数组

   for(let i=0; i<data.length,i++){
      if(data[i].endsWith(".jpg")){
          //console.log("进来的说明是jpg图片")
          jpg.push(data[i]);
          fs.readFile("./img/"+data[i],(err,file) => {
            fs.appendFile("./jpg/"+"故人居"+i+"."+data[i].split(".")[1],file,(err)=>{
              if(err) return console.log(err.message)  
            })
          })
      }
      if(data[i].endsWith(".png")){
          //console.log("进来的说明是png图片")
            //png.push(data[i]);
            fs.readFile("./img/"+data[i],(err,file) => {
            fs.appendFile("./png/"+"新人社"+i+"."+data[i].split(".")[1],file,(err)=>{
              if(err) return console.log(err.message);
            })
          })
      }
   }
})
```


当然，上面还是可以精简一些的，利用 `fs.copyFile` 可以省略掉一步，找到对应的图片信息，然后直接copy到对应的文件夹内，并对其进行改名操作。

### `fs` 文件模块 路径问题

 - ` __dirname ` **可以获得当前文件的路径，但是不包括当前文件的文件名。**
 
 ```javascript
    console.log(__dirname);
 ```


![](https://i.imgur.com/CZ9t31U.png)![](https://i.imgur.com/FqUgZ7p.png)

 - `path.join` ：**这个方法可以分辨 `./` 以及 `../`，可以将路径拼接，在 window系统的上显示的是 `\` 分割，在 Mac 等系统中，显示的是 `/` 分割。**
 
  ```javascript
    const psth = require("path");  //要使用 path 模块，需要先引入。
    console.log('c:/', 'a', './b', '/c', './d/e', 'f', '../g')  //    c:\a\b\c\d\e\g；
  ```


 用法：**主要用来在使用 `fs` 文件等模块...操作时，不再依赖相对路径。这样就算运行的文件移动了，那么也不会出错。**

 ```javascript
  const path = require("psth");
  const fs = require("fs");
  fs.readFile(path.join(__dirname,"./img"),function(err,data){
      if(err) return console.log(err.message);
      console.log(data);
  })
 ```

 - **`path.extname` ：可以获得当前文件的扩展名。**
  
 ```javascript
    const path = require("path")
    const str  = path.join(__dirname,"./1.txt");
    console.log(path.extname(Str)) // 输出结果为： .txt
 ```


 - **`path.basename` :可以获得指定文件的文件名，包括扩展名**
 
 ```javascript
    const path = require("path")
    const str  = path.join(__dirname,"./1.txt");
    console.log(path.basename(str))  //输出结果为： 1.txt
 ```