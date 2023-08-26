---
title: php-Basic
categories:
  - 编程
  - php
date: 2023-08-02 09:18:49
tags:
---

# PHP编程

> ~~php是世界上最好的编程语言！~~

PHP 是服务器端脚本语言。

- PHP（全称：PHP：Hypertext Preprocessor，即"PHP：超文本预处理器"）是一种通用开源脚本语言。
- PHP 脚本在服务器上执行。
- PHP 可免费下载使用。

- PHP 文件可包含文本、HTML、JavaScript代码和 PHP 代码
- PHP 代码在服务器上执行，结果以纯 HTML 形式返回给浏览器
- PHP 文件的默认文件扩展名是 ".php"

- PHP 可以生成动态页面内容
- PHP 可以创建、打开、读取、写入、关闭服务器上的文件
- PHP 可以收集表单数据
- PHP 可以发送和接收 cookies
- PHP 可以添加、删除、修改您的数据库中的数据
- PHP 可以限制用户访问您的网站上的一些页面
- PHP 可以加密数据

通过 PHP，您不再限于输出 HTML。您可以输出图像、PDF 文件，甚至 Flash 电影。您还可以输出任意的文本，比如 XHTML 和 XML。

<!--more-->

## PHP语法

PHP 脚本可以放在文档中的任何位置。

PHP 脚本以 **\<?php**  开始，以 **?\>**  结束：

```php
<?php
// PHP 代码
?>
```

PHP 文件的默认文件扩展名是 ".php"。

PHP 文件通常包含 HTML 标签和一些 PHP 脚本代码。

一个简单的例子：

```php+HTML
<!DOCTYPE html>
<html>
<body>

<h1>My first PHP page</h1>

<?php
echo "Hello World!";
?>

</body>
</html>
```

![image-20230802092826310](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\编程\php\php.assets\image-20230802092826310.png)

## PHP变量

```php+HTML
<?php

$x=5;
$y=6;
$z=$x+$y;
echo "x + y = $z"

?>
```

![image-20230802093043527](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\编程\php\php.assets\image-20230802093043527.png)

与代数类似，可以给 PHP 变量赋予某个值（`x=5`）或者表达式（`z=x+y`）。

PHP 变量规则：

- 变量以 $ 符号开始，后面跟着变量的名称
- 变量名必须以字母或者下划线字符开始
- 变量名只能包含字母、数字以及下划线（A-z、0-9 和 _ ）
- 变量名不能包含空格
- 变量名是区分大小写的（`$y` 和` $Y `是两个不同的变量）

> PHP 语句和 PHP 变量都是区分大小写的。

## PHP 变量作用域

变量的作用域是脚本中变量可被引用/使用的部分。

PHP 有四种不同的变量作用域：

- local
- global
- static
- parameter

## PHP echo 和 print 语句

`echo `和 `print `区别:

- `echo` - 可以输出一个或多个字符串，是一个语言结构，使用的时候可以不用加括号，也可以加上括号： `echo` 或 `echo()`。
- `print` - 只允许输出一个字符串，返回值总为 1，同样是一个语言结构，可以使用括号，也可以不使用括号：` print` 或 `print()`。

> echo 输出的速度比 print 快， echo 没有返回值，print有返回值1。

使用 `echo, print` 命令输出字符串可以包含 HTML 标签。

## PHP EOF(heredoc) 使用说明

PHP EOF(heredoc)是一种在命令行shell（如sh、csh、ksh、bash、PowerShell和zsh）和程序语言（像Perl、PHP、Python和Ruby）里定义一个字符串的方法。

使用概述：

- 必须后接分号，否则编译通不过。
- **EOF** 可以用任意其它字符代替，只需保证结束标识与开始标识一致。
- **结束标识必须顶格独自占一行(即必须从行首开始，前后不能衔接任何空白和字符)。**
- 开始标识可以不带引号或带单双引号，不带引号与带双引号效果一致，解释内嵌的变量和转义符号，带单引号则不解释内嵌的变量和转义符号。
- 当内容需要内嵌引号（单引号或双引号）时，不需要加转义符，**本身对单双引号转义**，此处相当与q和qq的用法。

```
<?php
$name="C4IN";
$a=<<<EOF
		name: "$name"
		 , hello
	EOF;
echo $a
?>
```

![image-20230802094636331](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\编程\php\php.assets\image-20230802094636331.png)

> **注意：**
>
> 1.以` <<<EOF` 开始标记开始，以 `EOF` 结束标记结束，结束标记必须顶头写，不能有缩进和空格，且在结束标记末尾要有分号 。
>
> 2.开始标记和结束标记相同，比如常用大写的 **EOT、EOD、EOF** 来表示，但是不只限于那几个(也可以用：JSON、HTML等)，只要保证开始标记和结束标记不在正文中出现即可。
>
> 3.位于开始标记和结束标记之间的变量可以被正常解析，但是函数则不可以。在 heredoc 中，变量不需要用连接符 **.** 或 **,** 来拼接。

# PHP 数据类型

PHP 变量存储不同的类型的数据，不同的数据类型可以做不一样的事情。

PHP 支持以下几种数据类型:

- String（字符串）
- Integer（整型）
- Float（浮点型）
- Boolean（布尔型）
- Array（数组）
- Object（对象）
- NULL（空值）
- Resource（资源类型）

### PHP 资源类型

PHP 资源 resource 是一种特殊变量，保存了到外部资源的一个引用。

常见资源数据类型有打开文件、数据库连接、图形画布区域等。

由于资源类型变量保存有为打开文件、数据库连接、图形画布区域等的特殊句柄，因此将其它类型的值转换为资源没有意义。

使用 `get_resource_type()` 函数可以返回资源（resource）类型：

```
get_resource_type(resource $handle): string
```

此函数返回一个字符串，用于表示传递给它的 resource 的类型。如果参数不是合法的 resource，将产生错误。

```
<?php
$c = mysql_connect();
echo get_resource_type($c)."\n";
// 打印：mysql link

$fp = fopen("foo","w");
echo get_resource_type($fp)."\n";
// 打印：file

$doc = new_xmldoc("1.0");
echo get_resource_type($doc->doc)."\n";
// 打印：domxml document
?>
```

# PHP 类型比较

虽然 PHP 是弱类型语言，但也需要明白变量类型及它们的意义，因为我们经常需要对 PHP 变量进行比较，包含松散和严格比较。

- 松散比较：使用两个等号 **==** 比较，只比较值，不比较类型。
- 严格比较：用三个等号 **===** 比较，除了比较值，也比较类型。

![img](https://www.runoob.com/wp-content/uploads/2019/05/1791863413-572055b100304_articlex.png)

![img](https://www.runoob.com/wp-content/uploads/2019/05/xxxxphp.png)

## PHP 常量

常量是一个简单值的标识符。该值在脚本中不能改变。

一个常量由英文字母、下划线、和数字组成,但数字不能作为首字母出现。 (常量名不需要加 $ 修饰符)。

**注意：** 常量在整个脚本中都可以使用。

### 设置 PHP 常量

设置常量，使用` define() `函数，函数语法如下：

```
bool define ( string $name , mixed $value [, bool $case_insensitive = false ] )
```

该函数有三个参数:

- **name：** 必选参数，常量名称，即标志符。

- **value：** 必选参数，常量的值。

- **case_insensitive**  ：可选参数，如果设置为 TRUE，该常量则大小写不敏感，默认是大小写敏感的。

  **注意：**自 PHP 7.3.0 开始，定义不区分大小写的常量已被弃用。从 PHP 8.0.0 开始，只有 false 是可接受的值，传递 true 将产生一个警告。

```
<?php
define('NAME','C4IN');
echo NAME;
echo C4IN;   // 输出，但会报常量未定义
?>
```

![image-20230802095340362](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\编程\php\php.assets\image-20230802095340362.png)

## PHP字符串

### PHP 并置运算符

在 PHP 中，只有一个字符串运算符。

并置运算符 (.) 用于把两个字符串值连接起来。

下面的实例演示了如何将两个字符串变量连接在一起：

```
<?php
$str1='name:';
$str2='C4IN';
$str3=$str1.' '.$str2;
echo $str3;
?>
```

![image-20230802095639820](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\编程\php\php.assets\image-20230802095639820.png)

### PHP `strlen()` 

`strlen() `函数返回字符串的长度（字节数）。

### PHP `strpos() `

strpos() 函数用于在字符串内查找一个字符或一段指定的文本。

如果在字符串中找到匹配，该函数会返回第一个匹配的字符位置。如果未找到匹配，则返回 FALSE。

```
<?php
echo strpos("Hello world!","world");
?>
```

## PHP运算符

### PHP 算术运算符

| 运算符 | 名称             | 描述                                                         | 实例                         | 结果  |
| :----- | :--------------- | :----------------------------------------------------------- | :--------------------------- | :---- |
| x + y  | 加               | x 和 y 的和                                                  | 2 + 2                        | 4     |
| x - y  | 减               | x 和 y 的差                                                  | 5 - 2                        | 3     |
| x * y  | 乘               | x 和 y 的积                                                  | 5 * 2                        | 10    |
| x / y  | 除               | x 和 y 的商                                                  | 15 / 5                       | 3     |
| x % y  | 模（除法的余数） | x 除以 y 的余数                                              | 5 % 2 10 % 8 10 % 2          | 1 2 0 |
| -x     | 设置负数         | 取 x 的相反符号                                              | `<?php $x = 2; echo -$x; ?>` | -2    |
| ~x     | 取反             | x 取反，按二进制位进行"取反"运算。运算规则：`~1=-2;    ~0=-1;` | `<?php $x = 2; echo ~$x; ?>` | -3    |
| a . b  | 并置             | 连接两个字符串                                               | "Hi" . "Ha"                  | HiHa  |

> PHP7+ 版本新增整除运算符 `intdiv()`，该函数返回值为第一个参数除于第二个参数的值并取整（向下取整）。
>
> ```
> <?php
> var_dump(intdiv(10, 3));
> ?>
> ```
>
> ```
> int(3)
> ```

### PHP 赋值运算符

在 PHP 中，基本的赋值运算符是 **=**。它意味着左操作数被设置为右侧表达式的值。也就是说，**$x = 5** 的值是 5。

| 运算符 | 等同于    | 描述                           |
| :----- | :-------- | :----------------------------- |
| x = y  | x = y     | 左操作数被设置为右侧表达式的值 |
| x += y | x = x + y | 加                             |
| x -= y | x = x - y | 减                             |
| x *= y | x = x * y | 乘                             |
| x /= y | x = x / y | 除                             |
| x %= y | x = x % y | 模（除法的余数）               |
| a .= b | a = a . b | 连接两个字符串                 |

### PHP 递增/递减运算符

| 运算符 | 名称   | 描述                |
| :----- | :----- | :------------------ |
| ++ x   | 预递增 | x 加 1，然后返回 x  |
| x ++   | 后递增 | 返回 x，然后 x 加 1 |
| -- x   | 预递减 | x 减 1，然后返回 x  |
| x --   | 后递减 | 返回 x，然后 x 减 1 |

### PHP 比较运算符

比较操作符可以让您比较两个值：

| 运算符  | 名称       | 描述                                           | 实例               |
| :------ | :--------- | :--------------------------------------------- | :----------------- |
| x == y  | 等于       | 如果 x 等于 y，则返回 true                     | 5==8 返回 false    |
| x === y | 绝对等于   | 如果 x 等于 y，**且它们类型相同**，则返回 true | 5==="5" 返回 false |
| x != y  | 不等于     | 如果 x 不等于 y，则返回 true                   | 5!=8 返回 true     |
| x <> y  | 不等于     | 如果 x 不等于 y，则返回 true                   | 5<>8 返回 true     |
| x !== y | 不绝对等于 | 如果 x 不等于 y，或它们类型不相同，则返回 true | 5!=="5" 返回 true  |
| x > y   | 大于       | 如果 x 大于 y，则返回 true                     | 5>8 返回 false     |
| x < y   | 小于       | 如果 x 小于 y，则返回 true                     | 5<8 返回 true      |
| x >= y  | 大于等于   | 如果 x 大于或者等于 y，则返回 true             | 5>=8 返回 false    |
| x <= y  | 小于等于   | 如果 x 小于或者等于 y，则返回 true             | 5<=8 返回 true     |

### PHP 逻辑运算符

| 运算符   | 名称 | 描述                                         | 实例                                 |
| :------- | :--- | :------------------------------------------- | :----------------------------------- |
| x and y  | 与   | 如果 x 和 y 都为 true，则返回 true           | x=6 y=3 (x < 10 and y > 1) 返回 true |
| x or y   | 或   | 如果 x 和 y 至少有一个为 true，则返回 true   | x=6 y=3 (x\==6 or y\==5) 返回 true   |
| x xor y  | 异或 | 如果 x 和 y 有且仅有一个为 true，则返回 true | x=6 y=3 (x\==6 xor y==3) 返回 false  |
| x && y   | 与   | 如果 x 和 y 都为 true，则返回 true           | x=6 y=3 (x < 10 && y > 1) 返回 true  |
| x \|\| y | 或   | 如果 x 和 y 至少有一个为 true，则返回 true   | x=6 y=3 (x\==5 \|\| y==5) 返回 false |
| !x       | 非   | 如果 x 不为 true，则返回 true                | x=6 y=3, !(x==y) 返回 true           |

### PHP 数组运算符

| 运算符  | 名称   | 描述                                                         |
| :------ | :----- | :----------------------------------------------------------- |
| x + y   | 集合   | x 和 y 的集合                                                |
| x == y  | 相等   | 如果 x 和 y 具有相同的键/值对，则返回 true                   |
| x === y | 恒等   | 如果 x 和 y 具有相同的键/值对，且顺序相同类型相同，则返回 true |
| x != y  | 不相等 | 如果 x 不等于 y，则返回 true                                 |
| x <> y  | 不相等 | 如果 x 不等于 y，则返回 true                                 |
| x !== y | 不恒等 | 如果 x 不等于 y，则返回 true                                 |

## PHP 条件语句

在 PHP 中，提供了下列条件语句：

- **if 语句** - 在条件成立时执行代码
- **if...else 语句** - 在条件成立时执行一块代码，条件不成立时执行另一块代码
- **if...elseif....else 语句** - 在若干条件之一成立时执行一个代码块
- **switch 语句** - 在若干条件之一成立时执行一个代码块

## PHP数组

### 在 PHP 中创建数组

在 PHP 中，`array() `函数用于创建数组：

```
array();
```

在 PHP 中，有三种类型的数组：

- **数值数组** - 带有数字 ID 键的数组
- **关联数组** - 带有指定的键的数组，每个键关联一个值
- **多维数组** - 包含一个或多个数组的数组

### PHP 数值数组

这里有两种创建数值数组的方法：

自动分配 ID 键（ID 键总是从 0 开始）：

```
$cars=array("Volvo","BMW","Toyota");
```

人工分配 ID 键：

```
$cars[0]="Volvo";
$cars[1]="BMW";
$cars[2]="Toyota";
```

```
<?php
$cars=array("Volvo","BMW","Toyota");
echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
?>
```

![image-20230802101335205](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\编程\php\php.assets\image-20230802101335205.png)

### `count() `函数

`count() `函数用于返回数组的长度（元素的数量）。

### PHP 关联数组

关联数组是使用分配给数组的指定的键的数组。

这里有两种创建关联数组的方法：

```
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
```

or:

```
$age['Peter']="35";
$age['Ben']="37";
$age['Joe']="43";
```

随后可以在脚本中使用指定的键：

```
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
echo "Peter is " . $age['Peter'] . " years old.";
?>
```

#### 遍历关联数组

遍历并打印关联数组中的所有值，可以使用 foreach 循环，如下所示：

```
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
 
foreach($age as $x=>$x_value)
{
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
}
?>
```

![image-20230802102155193](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\编程\php\php.assets\image-20230802102155193.png)



## PHP - 数组排序函数·

- `sort() `- 对数组进行升序排列
- `rsort()` - 对数组进行降序排列
- `asort() `- 根据关联数组的值，对数组进行升序排列
- `ksort() `- 根据关联数组的键，对数组进行升序排列
- `arsort() `- 根据关联数组的值，对数组进行降序排列
- `krsort() `- 根据关联数组的键，对数组进行降序排列

## PHP 超级全局变量

PHP中预定义了几个超级全局变量（superglobals） ，这意味着它们在一个脚本的全部作用域中都可用。 你不需要特别说明，就可以在函数及类中使用。

PHP 超级全局变量列表:

- `$GLOBALS`

  ```
  <?php 
  $x = 75; 
  $y = 25;
   
  function addition() 
  { 
      $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y']; 
  }
   
  addition(); 
  echo $z; 
  ?>
  ```

  z 是一个`$GLOBALS`数组中的超级全局变量，该变量同样可以在函数外访问。

- `$_SERVER`

  `$_SERVER` 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目**由 Web 服务器创建**。不能保证每个服务器都提供全部项目；服务器可能会忽略一些，或者提供一些没有在这里列举出来的项目。

  以下实例中展示了如何使用`$_SERVER`中的元素：

  ```
  <?php 
  echo $_SERVER['PHP_SELF'];
  echo "<br>";
  echo $_SERVER['SERVER_NAME'];
  echo "<br>";
  echo $_SERVER['HTTP_HOST'];
  echo "<br>";
  echo $_SERVER['HTTP_REFERER'];
  echo "<br>";
  echo $_SERVER['HTTP_USER_AGENT'];
  echo "<br>";
  echo $_SERVER['SCRIPT_NAME'];
  ?>
  ```

  | 元素/代码                       | 描述                                                         |
  | :------------------------------ | :----------------------------------------------------------- |
  | $_SERVER['PHP_SELF']            | 当前执行脚本的文件名，与 document root 有关。例如，在地址为 http://example.com/test.php/foo.bar 的脚本中使用 $_SERVER['PHP_SELF'] 将得到 /test.php/foo.bar。__FILE__ 常量包含当前(例如包含)文件的完整路径和文件名。 从 PHP 4.3.0 版本开始，如果 PHP 以命令行模式运行，这个变量将包含脚本名。之前的版本该变量不可用。 |
  | $_SERVER['GATEWAY_INTERFACE']   | 服务器使用的 CGI 规范的版本；例如，"CGI/1.1"。               |
  | $_SERVER['SERVER_ADDR']         | 当前运行脚本所在的服务器的 IP 地址。                         |
  | $_SERVER['SERVER_NAME']         | 当前运行脚本所在的服务器的主机名。如果脚本运行于虚拟主机中，该名称是由那个虚拟主机所设置的值决定。(如: www.runoob.com) |
  | $_SERVER['SERVER_SOFTWARE']     | 服务器标识字符串，在响应请求时的头信息中给出。 (如：Apache/2.2.24) |
  | $_SERVER['SERVER_PROTOCOL']     | 请求页面时通信协议的名称和版本。例如，"HTTP/1.0"。           |
  | $_SERVER['REQUEST_METHOD']      | 访问页面使用的请求方法；例如，"GET", "HEAD"，"POST"，"PUT"。 |
  | $_SERVER['REQUEST_TIME']        | 请求开始时的时间戳。从 PHP 5.1.0 起可用。 (如：1377687496)   |
  | $_SERVER['QUERY_STRING']        | query string（查询字符串），如果有的话，通过它进行页面访问。 |
  | $_SERVER['HTTP_ACCEPT']         | 当前请求头中 Accept: 项的内容，如果存在的话。                |
  | $_SERVER['HTTP_ACCEPT_CHARSET'] | 当前请求头中 Accept-Charset: 项的内容，如果存在的话。例如："iso-8859-1,*,utf-8"。 |
  | $_SERVER['HTTP_HOST']           | 当前请求头中 Host: 项的内容，如果存在的话。                  |
  | $_SERVER['HTTP_REFERER']        | 引导用户代理到当前页的前一页的地址（如果存在）。由 user agent 设置决定。并不是所有的用户代理都会设置该项，有的还提供了修改 HTTP_REFERER 的功能。简言之，该值并不可信。) |
  | $_SERVER['HTTPS']               | 如果脚本是通过 HTTPS 协议被访问，则被设为一个非空的值。      |
  | $_SERVER['REMOTE_ADDR']         | 浏览当前页面的用户的 IP 地址。                               |
  | $_SERVER['REMOTE_HOST']         | 浏览当前页面的用户的主机名。DNS 反向解析不依赖于用户的 REMOTE_ADDR。 |
  | $_SERVER['REMOTE_PORT']         | 用户机器上连接到 Web 服务器所使用的端口号。                  |
  | $_SERVER['SCRIPT_FILENAME']     | 当前执行脚本的绝对路径。                                     |
  | $_SERVER['SERVER_ADMIN']        | 该值指明了 Apache 服务器配置文件中的 SERVER_ADMIN 参数。如果脚本运行在一个虚拟主机上，则该值是那个虚拟主机的值。(如：someone@runoob.com) |
  | $_SERVER['SERVER_PORT']         | Web 服务器使用的端口。默认值为 "80"。如果使用 SSL 安全连接，则这个值为用户设置的 HTTP 端口。 |
  | $_SERVER['SERVER_SIGNATURE']    | 包含了服务器版本和虚拟主机名的字符串。                       |
  | $_SERVER['PATH_TRANSLATED']     | 当前脚本所在文件系统（非文档根目录）的基本路径。这是在服务器进行虚拟到真实路径的映像后的结果。 |
  | $_SERVER['SCRIPT_NAME']         | 包含当前脚本的路径。这在页面需要指向自己时非常有用。__FILE__ 常量包含当前脚本(例如包含文件)的完整路径和文件名。 |
  | $_SERVER['SCRIPT_URI']          | URI 用来指定要访问的页面。例如 "/index.html"。               |

- `$_REQUEST`

   `$_REQUEST `用于收集HTML表单提交的数据。

  以下实例显示了一个输入字段（input）及提交按钮(submit)的表单(form)。 当用户通过点击 "Submit" 按钮提交表单数据时, 表单数据将发送至`<form>`标签中 action 属性中指定的脚本文件。 在这个实例中，我们指定文件来处理表单数据。如果你希望其他的PHP文件来处理该数据，你可以修改该指定的脚本文件名。 然后，我们可以使用超级全局变量 $_REQUEST 来收集表单中的 input 字段数据：

  ```
  <html>
  <body>
   
  <form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
  Name: <input type="text" name="fname">
  <input type="submit">
  </form>
   
  <?php 
  $name = $_REQUEST['fname']; 
  echo $name; 
  ?>
   
  </body>
  </html>
  ```

- `$_POST`

   `$_POST `被广泛应用于收集表单数据，在HTML form标签的指定该属性：`"method="post"`。

  以下实例显示了一个输入字段（input）及提交按钮(submit)的表单(form)。 当用户通过点击 "Submit" 按钮提交表单数据时, 表单数据将发送至`<form>`标签中 action 属性中指定的脚本文件。 在这个实例中，我们指定文件来处理表单数据。如果你希望其他的PHP文件来处理该数据，你可以修改该指定的脚本文件名。 然后，我们可以使用超级全局变量 $_POST 来收集表单中的 input 字段数据：

  ```
  <html>
  <body>
   
  <form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
  Name: <input type="text" name="fname">
  <input type="submit">
  </form>
   
  <?php 
  $name = $_POST['fname']; 
  echo $name; 
  ?>
   
  </body>
  </html>
  ```

- `$_GET`

  `$_GET `同样被广泛应用于收集表单数据，在HTML form标签的指定该属性：`"method="get"`。

  $_GET 也可以收集URL中发送的数据。

  假定我们有一个包含参数的超链接HTML页面：

  ```
  <html>
  <body>
  
  <a href="test_get.php?subject=PHP&web=runoob.com">Test $GET</a>
  
  </body>
  </html>
  ```

  当用户点击链接 "Test \$GET", 参数 "subject" 和 "web" 将发送至"test_get.php",你可以在 "test_get.php" 文件中使用 $_GET 变量来获取这些数据。

- `$_FILES`

- `$_ENV`

- `$_COOKIE`

- `$_SESSION`

## PHP 函数

### 创建 PHP 函数

函数是通过调用函数来执行的。

```
<?php
function functionName()
{
    // 要执行的代码
}
?>
```

PHP 函数准则：

- 函数的名称应该提示出它的功能
- 函数名称以字母或下划线开头（不能以数字开头）

### 添加参数

为了给函数添加更多的功能，我们可以添加参数，参数类似变量。

参数就在函数名称后面的一个括号内指定。

```
<?php
function writeName($fname)
{
    echo $fname . " Refsnes.<br>";
}
 
echo "My name is ";
writeName("Kai Jim");
echo "My sister's name is ";
writeName("Hege");
echo "My brother's name is ";
writeName("Stale");
?>
```

### 返回值

使用 return 语句。

```
<?php
function add($x,$y)
{
    $total=$x+$y;
    return $total;
}
 
echo "1 + 16 = " . add(1,16);
?>
```

## PHP 魔术常量

PHP 向它运行的任何脚本提供了大量的预定义常量。

不过很多常量都是由不同的扩展库定义的，只有在加载了这些扩展库时才会出现，或者动态加载后，或者在编译时已经包括进去了。

有八个魔术常量它们的值随着它们在代码中的位置改变而改变。

### \__LINE__

文件中的当前行号。

```
<?php
echo '这是第 " '  . __LINE__ . ' " 行';
?>
```

### \__FILE__

文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。

自 PHP 4.0.2 起，__FILE__ 总是包含一个绝对路径（如果是符号连接，则是解析后的绝对路径），而在此之前的版本有时会包含一个相对路径。

```
<?php
echo '该文件位于 " '  . __FILE__ . ' " ';
?>
```

### \__DIR__

文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录。

它等价于 dirname(__FILE__)。除非是根目录，否则目录中名不包括末尾的斜杠。（PHP 5.3.0中新增）

```
<?php
echo '该文件位于 " '  . __DIR__ . ' " ';
?>
```

### \__FUNCTION__

函数名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该函数被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。

```
<?php
function test() {
    echo  '函数名为：' . __FUNCTION__ ;
}
test();
?>
```

### \__CLASS__

类的名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该类被定义时的名字（区分大小写）。

在 PHP 4 中该值总是小写字母的。类名包括其被声明的作用区域（例如 Foo\Bar）。注意自 PHP 5.4 起 __CLASS__ 对 trait 也起作用。当用在 trait 方法中时，__CLASS__ 是调用 trait 方法的类的名字。

```
<?php
class test {
    function _print() {
        echo '类名为：'  . __CLASS__ . "<br>";
        echo  '函数名为：' . __FUNCTION__ ;
    }
}
$t = new test();
$t->_print();
?>
```

### \__TRAIT__

Trait 的名字（PHP 5.4.0 新加）。自 PHP 5.4.0 起，PHP 实现了代码复用的一个方法，称为 traits。

Trait 名包括其被声明的作用区域（例如 Foo\Bar）。

从基类继承的成员被插入的 SayWorld Trait 中的 MyHelloWorld 方法所覆盖。其行为 MyHelloWorld 类中定义的方法一致。优先顺序是当前类中的方法会覆盖 trait 方法，而 trait 方法又覆盖了基类中的方法。

```
<?php
class Base {
    public function sayHello() {
        echo 'Hello ';
    }
}
 
trait SayWorld {
    public function sayHello() {
        parent::sayHello();
        echo 'World!';
    }
}
 
class MyHelloWorld extends Base {
    use SayWorld;
}
 
$o = new MyHelloWorld();
$o->sayHello();
?>
```

### \__METHOD__

类的方法名（PHP 5.0.0 新加）。返回该方法被定义时的名字（区分大小写）。

```
<?php
function test() {
    echo  '函数名为：' . __METHOD__ ;
}
test();
?>
```

### \__NAMESPACE__

当前命名空间的名称（区分大小写）。此常量是在编译时定义的（PHP 5.3.0 新增）。

## 一些文章

https://www.runoob.com/php/php-intro.html
