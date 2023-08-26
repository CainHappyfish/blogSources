---
title: php序列化与反序列化
tags: CTF
categories:
  - CTF
  - web
date: 2023-08-01 17:26:29
---

# 序列化与反序列化

为了方便数据存储,php通常会将数组等数据转换为序列化形式存储，那么什么是序列化呢？序列化其实就是将数据转化成一种可逆的数据结构，自然，逆向的过程就叫做反序列化。

网上有一个形象的例子，这个例子会让我们深刻的记住**序列化的目的是方便数据的传输和存储**。而在PHP中，序列化和反序列化一般用做缓存，比如session缓存，cookie等。

> 现在我们都会在淘宝上买桌子，桌子这种很不规则的东西，该怎么从一个城市运输到另一个城市，这时候一般都会把它拆掉成板子，再装到箱子里面，就可以快递寄出去了，这个过程就类似我们的序列化的过程（把数据转化为可以存储或者传输的形式）。当买家收到货后，就需要自己把这些板子组装成桌子的样子，这个过程就像反序列的过程（转化成当初的数据对象）。

下面附一张形象的过程图。

![image-20230802113330751](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\web\php-unserialize.assets\image-20230802113330751.png)

<!--more-->

### MAGIC方法

PHP 的面向对象中包含一些魔术方法，这些方法在某种情况下会被自动调用。

| magic 方法       | 功能                                               |
| ---------------- | -------------------------------------------------- |
| `__construct()`  | 类构造器                                           |
| `__destruct()`   | 类的析构器                                         |
| `__sleep()`      | 执行 `serialize() `时，先会调用这个函数            |
| `__wakeup()`     | 执行` unserialize() `时，先会调用这个函数          |
| `__toString()`   | 类被当成字符串时的回应方法                         |
| `__invoke()`     | 当尝试以调用函数的方法调用一个对象时，会被自动调用 |
| `__call()`       | 在对象上下文中调用不可访问的方法时触发             |
| `__callStatic()` | 在静态上下文中调用不可访问的方法时触发             |
| `__get()`        | 用于从不可访问的属性读取数据                       |
| `__set()`        | 用于将数据写入不可访问的属性                       |
| `__isset()`      | 在不可访问的属性上调用`isset()`或`empty()`触发     |
| `__unset()`      | 在不可访问的属性上使用`unset()`时触发              |

`unserialize()`函数的变量可控，文件中存在可利用的类，类中有魔术方法：

### 魔术方法绕过

https://pankas.top/2022/08/04/php(phar)%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E6%BC%8F%E6%B4%9E%E5%8F%8A%E5%90%84%E7%A7%8D%E7%BB%95%E8%BF%87%E5%A7%BF%E5%8A%BF/#%E5%90%84%E7%A7%8D%E7%BB%95%E8%BF%87%E5%A7%BF%E5%8A%BF

### PHP反序列化

> `new`实例化对象时构造函数需要参数的话必须要加括号，无参数的话可加可不加，因此建议都加括号，也就是`new class();`

> 如`class student()`，这里面如果有参数`$name`,则必须加括号，没有的话可加可不加

PHP对象需要表达的内容较多，有一个基本类型表达的基本格式，大体分为六个类型。

| 布尔值（bool）   | b:value                       | 例：b:0             |
| :--------------- | :---------------------------- | ------------------- |
| 整数型（int）    | `i:value`                     | `i:1`               |
| 字符串型（str）  | `s:length`:"value"            | `s:4:"aaaa"`        |
| 数组型（array）  | `a:length:{key:value pairs};` | `a:1:{i:1;s:1:"a"}` |
| 对象型（object） | `O:<class_name_length>`       |                     |
| NULL型           | N                             |                     |

```
a - array
b - boolean
d - double
i - integer
o - common object
r - reference
s - string
C - custom object
O - class
N - null
R - pointer reference
U - unicode string
```

e.g. 序列化

```
<?php class message{
    public $from='d';
    public $msg='m';
    public $to='1';
    public $token='user';
    }
$msg= serialize(new message);
print_r($msg);
```

![image-20230802111643317](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\web\php-unserialize.assets\image-20230802111643317.png)

序列化后的内容只有成员变量，没有成员函数：

```
<?php
class test{
    public $a;
    public $b;
    function __construct(){$this->a = "Cain";$this->b="fish";}
    function happy(){return $this->a;}
}
$a = new test();
echo serialize($a);
?>
```

![image-20230802112453559](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\web\php-unserialize.assets\image-20230802112453559.png)

`construct()`和`happy()`函数包括函数里面的类不会被输出。

在 PHP 中，序列化对象时，public、protected 和 private 属性会被编码为特定的格式。这是 PHP 的内部机制，用于区分不同的属性可见性。

- public属性：直接编码为属性名。
- protected属性：编码为 "\0*\0" + 属性名。
- private属性：编码为 "\0" + 类名 + "\0" + 属性名。

这些特殊编码并不是为了被浏览器直接读取，而是为了在 PHP 内部正确处理对象的序列化和反序列化。当你在 PHP 中反序列化这个字符串时，PHP 会根据这些特殊编码恢复出原来的对象。

当遇到特殊编码时，其实只需要用php自带的编码解码工具转换成我们需要的东西就可以了。

### 一些文章

https://cloud.tencent.com/developer/article/2121687

https://www.cnblogs.com/linfangnan/p/13520608.html

https://www.php.cn/faq/481558.html

https://fushuling.com/index.php/2023/03/11/php%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E4%B8%ADwakeup%E7%BB%95%E8%BF%87%E6%80%BB%E7%BB%93/
