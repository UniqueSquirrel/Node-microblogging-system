# 基于Node和MongoDB的Web微博系统

## 概述
 项目是受到《Node.js开发指南》一书启发，作者BYVoid大佬是当时第一批Node开发者，当时学习者迫切需要一个这么DEMO。当然现在已经过去很多年了，Node早就更新无数次了，12年的这本书也就过时，许多代码都根本跑不通了，最后还是要以官网API为准。
 1. 项目的后台用的express。
 2. 数据库部分自己写的原生MongoDB，没有去用控制力更强大的mongoose框架，多写原生东西对自己有好处。
 3. 前端用的是jQuery,underscore.js
 4. 样式用的Bootstrap框架。
 
## 设计
 形式上采用的是MVC结构，数据库和加密部分在model层，路由控制充当Controller，ejs前端展示就是view层了。
 
## 安装依赖
    "dependencies": {
      "ejs": "^2.5.7",
      "express": "^4.16.2",
      "express-session": "^1.15.6",
      "formidable": "^1.1.1",
      "gm": "^1.23.0",
      "mongodb": "^2.2.33" }
    
如果想二次开发，node——modules部分建议按照上面版本自己重新npm install，我的node版本是 v8.3.0 ，目前最新的是v8.9.0

## 功能和实现
 1. 登陆模块，注册模块，微博的发表，全部用户的微博，个人全部微博，所有用户列表，点击某个用户可以看到该用户发表过的微博，用户退出，头像的上传和剪裁，登陆之后直接点击自己的默认头像可进入头像上传页面，主页根据登陆状态变化呈现不同页面。
 2. 状态的判断是通过express-session来传递。
 3. 服务器对头像图片的处理用到了GraphicsMagick ，这个插件可以截屏，然后我写了一个脚本获取到这个插件截屏时大小参数，在前端界面上进行图片的剪裁。
 4. 数据库对mongo的增删改查和分页功能比较原生，直接参考官网Mongo对Nodejs开放的API,一些操作实现了重载化，JS里是没有函数重载的，只能自己手写判断，比如查询操作find函数，可以根据参数的不同进行重载，根据参数的个数判断，来确实是不是要对查询的结果进行排序，分页，跳过和限制。
 5. 微博的展示部分，用到了underscore.js，由于和ejs的模版标记重合了，我修改了underscore的源码，把模版标记<%%>变成了{{}}。全部微博展示的处理中，因为用户的信息是存在user集合里，微博存在posts集合里，mongo不像mysql传统关系型数据库那样，是没有外键的，我get所有微博的时候查询的是posts集合，posts集合里没有存用户的头像，我要想在全部微博里显示用户的头像，只好采用双重AJAX回调，第一层获取到所有说说，第二层在迭代器中，这个迭代器是一个递归，为了分页效果，用迭代器实现了闭包，这是JS的坑谁用谁知道。(当然ES6可以轻松解决这个问题)，迭代器获得第一层回调结果的username，再查询获取到头像，再合并到第一个查询结果。
 6. 还有其他的一些东西什么的感兴趣可以fork，clone 看源码。

## 待改进
 微博的评论，点赞功能，每个微博的主页功能等等，能填的坑还有很多，不过大体框架写好了，开源出来也是希望大家能一起加功能。
 
## 结语
这个项目用来练习Node比较好了，代码注释写的很多，如果本项目对您有一丝丝的小帮助，求个star，也欢迎fork，一起加上更多功能。

## DEMO
![image](https://github.com/ZhangMingZhao1/Node-microblogging-system/blob/master/demo.jpg)


# 有的问题可以Issue，或者加扣扣1104272319联系我。

 
