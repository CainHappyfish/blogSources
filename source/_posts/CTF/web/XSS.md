---
title: XSS
categories:
  - CTF
  - web
date: 2023-07-29 22:00:25
tags: 
 - wow
---

# XSS

Cross-Site Scripting 简称为“CSS”，为避免与前端叠成样式表的缩写"CSS"冲突，故又称XSS。一般XSS可以分为如下几种常见类型：

1. 反射性XSS;
2. 存储型XSS;
3. DOM型XSS;

XSS漏洞一直被评估为web漏洞中危害较大的漏洞，在OWASP TOP10的排名中一直属于前三。
XSS是一种发生在前端浏览器端的漏洞，所以其危害的对象也是前端用户。XSS漏洞可以用来进行钓鱼攻击、钓鱼攻击、前端js挖矿、用户cookie获取。甚至可以结合浏览器自身的漏洞对用户主机进行远程控制等。
形成XSS漏洞的主要原因是程序对输入和输出没有做合适的处理，导致“精心构造”的字符输出在前端时被浏览器当作有效代码解析执行从而产生危害。

<!--more-->

本文主要以pikachu靶场进行说明。

[ pikachu靶场通关](https://blog.csdn.net/qq_53571321/article/details/121692906)

### XSS类型

- 反射型
  交互的数据一般不会被存在在数据库里面,一次性,所见即所得，一般出现在查询类页面等。

- 存储型
  交互的数据会被存在在数据库里面，永久性存储，一般出现在留言板，注册等页面。

- DOM型
  不与后台服务器产生数据交互,是一种通过DOM操作前端代码输出的时候产生的问题,一次性也属于反射型。

#### 跨站脚本漏洞测试流程

1. 在目标站点上找到输入点,比如查询接口,留言板等;
2. 输入一组”特殊字符+唯一识别字符”, 点击提交后,查看返回的源码，是否有做对应的处理;
   ③通过搜索定位到唯一字符，结合唯一字符前后语法确认是否可以构造执行js的条件 (构造闭合) ;
   ④提交构造的脚本代码(以及各种绕过姿势)，看是否可以成功执行，如果成功执行则说明存在XSS漏洞;

> 提示：
>
> 1. 一般查询接口容易出现反射型XSS ,留言板容易出现存储型XSS ;
> 2. 由于后台可能存在过滤措施，构造的script可能会被过滤掉,而无法生效或者环境限制了执行(浏览器) ;
> 3. 通过变化不同的script ,尝试绕过后台过滤机制;

### 反射型XSS

#### GET型

在对应的输入点输入特殊字符如：`;"’<>9999`
目的就是看一看是否会被过滤，（因为我们含有特殊字符）有没有被输出，有没有被处理。点击提交。



发现敏感字符没有被过滤。尝试：

语句输入到一半时，发现不能输入了，原因是对长度进行了限制。这是在前端的安全设置，意义不是很大。兵来将挡水来土掩，在设置中打开web开发者工具，查找输入框语句，修改输入长度。

![image-20230731140626403](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\web\XSS.assets\image-20230731140626403.png)

![image-20230731140635442](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\web\XSS.assets\image-20230731140635442.png)

xss攻击成功。当你刷新页面，弹窗消失。

#### POST型



### 存储型XSS

存储型XSS漏洞跟反射型形成的原因一样,不同的是存储型XSS下攻击者可以将脚本注入到后台存储起来，构成更加持久的危害,因此存储型XSS也称“永久型”XSS。

![image-20230731141438517](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\web\XSS.assets\image-20230731141438517.png)

发现我们的输入被储存，按之前的思路，测这个留言板是否存在xss漏洞，在留言板输入特殊字符。

![image-20230731141834706](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\web\XSS.assets\image-20230731141834706.png)

发现没有被过滤。我们还是输入一个payload：`<script>alert('xss')</script>`。

![image-20230731142217314](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\web\XSS.assets\image-20230731142217314.png)

进行页面切换或者刷新，发现弹窗依然存在，说明我们输入的语句已经被存储起来。

### DOM型XSS

#### Dom

[什么是DOM](https://blog.csdn.net/m0_58620483/article/details/126499977?ops_request_misc=%7B%22request%5Fid%22%3A%22169078478116800226592812%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=169078478116800226592812&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-126499977-null-null.142^v91^control_2,239^v12^control2&utm_term=DOM&spm=1018.2226.3001.4187)

[ 关于DOM型XSS的深入解析](https://blog.csdn.net/qq_53577336/article/details/122441536?ops_request_misc=%7B%22request%5Fid%22%3A%22169078545416800226591807%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=169078545416800226591807&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-122441536-null-null.142^v91^control_2,239^v12^control2&utm_term=DOM-XSS&spm=1018.2226.3001.4187)

DOM是JS操作网页的接口，全称为“文档对象模型”（Document Object Model）。它的作用是将网页转为一个JS对象，从而可以用脚本进行各种操作（比如增删内容）。

> - 文档（Document）
>
>   文档表示的就是整个的HTML网页文档
>
> -  对象（Object）
>
>   对象表示将网页中的每一个部分都转换为了一个对象。
>
> - 模型（Model）
>
>   使用模型来表示对象之间的关系，这样方便我们获取对象

文档对象模型（DOM）是网页的编程接口。它给文档（结构树）提供了一个结构化的表述并且定义了一种方式——程序可以对结构树进行访问，以改变文档的结构，样式和内容。

DOM提供了一种表述形式将文档作为一个结构化的节点组以及包含属性和方法的对象。从本质上说，它将web页面和脚本或编程语言连接起来了。

要改变页面的某个东西，JS就需要获得对网页中所有元素进行访问的入口。这个入口，连同对HTML元素进行添加、移动、改变或移除的方法和属性，都是通过DOM来获得的。

浏览器会根据DOM模型，将结构化文档（比如HTML和XML）解析成一系列的节点，再由这些节点组成一个树状结构（DOM Tree）。所有的节点和最终的树状结构，都有规范的对外接口。所以，DOM可以理解成网页的编程接口。DOM有自己的国际标准，目前的通用版本是DOM 3，DOM 4。

![ ](https://img-blog.csdnimg.cn/9bb1631414374b5bad883e949c8259ad.png?x-oss-process=image)

所以说DOM型xss可以在前端通过js渲染来完成数据的交互，达到插入数据造成xss脚本攻击，且不经过服务器，所以即使抓包无无法抓取到这里的流量，而反射性与存储型xss需要与服务器交互，这便是三者的区别。
