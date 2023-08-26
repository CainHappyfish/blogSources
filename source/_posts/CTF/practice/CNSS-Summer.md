---
title: CNSS-Summer
categories:
  - CTF
  - practice
date: 2023-08-01 09:25:33
tags: 
- CTF 

---

CNSS-Summer自己的题解。

<!--more-->

# WEB

## Backdoor

`eval`远程代码执行。

输入`system('ls /');`查看当前文件夹，发现flag。

![image-20230801094738133](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801094738133.png)

![image-20230801094653646](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801094653646.png)

## Webpack

Webpack漏洞挖掘。

![image-20230801093401794](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801093401794.png)

`ctrl+U`查看网页`js`源码

![image-20230801093423736](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801093423736.png)

加上后缀`.map`下载到本地，使用`npm i --global reverse-sourcemap`安装逆向工具，并使用

```
reverse-sourcemap --output-dir ./ main.c91fb7d1.js.map
```

进行文件还原。

![image-20230801093618036](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801093618036.png)

![image-20230801093830796](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801093830796.png)

发现flag。

##  Leak

看到vim，开始以为存在`git`泄漏，然后发现不得行。~~后面发现直接读取就完了~~

![image-20230801095935443](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801095935443.png)

在vim的缓存中这些可见字符会原样保留。再将该swp文件复原得到：

![image-20230801100528638](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801100528638.png)

然后是RCE了。

![image-20230801101046090](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801101046090.png)

![image-20230801101112101](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801101112101.png)

试试`curl`：

![image-20230801100208673](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801100208673.png)

## ezhttp

查看源代码，发现：

![image-20230801103928759](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801103928759.png)

把请求方式改成CNSS，然后

![image-20230801105202637](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801105202637.png)

上网查了一下安卓微信的内置浏览器User-Agent，继续：

![image-20230801105406978](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801105406978.png)

![image-20230801105416572](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801105416572.png)

然后将`referer`改成cnss.io:

![image-20230801114315983](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801114315983.png)

![image-20230801114319854](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801114319854.png)

> `referer`：包含一个URL，用户从该URL代表的页面出发访问当前请求的页面

使用`X-Forwarded-For`伪造ip：

![image-20230801114407481](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801114407481.png)

![image-20230801114358251](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801114358251.png)

> `X-Forwarded-For`是一个常见的HTTP请求头字段，用于表示客户端的原始IP地址。它通常被代理服务器或负载均衡器添加到请求头中，以便在请求转发过程中传递客户端的真实IP地址。
>
> 当请求通过代理服务器或负载均衡器转发时，原始的客户端IP地址会被替换为代理服务器的IP地址。为了保留原始客户端的IP地址，代理服务器会将原始IP地址添加到`X-Forwarded-For`字段中，并将其包含在请求头中。

将主机名`Host`改为uestc.edu.cn

![image-20230801130654931](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801130654931.png)

![image-20230801130714658](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801130714658.png)

> `Host`：指定发起请求的服务器的域名和端口号。

![image-20230801131425895](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801131425895.png)



> `Content-Type` 是 HTTP 请求头和响应头中的一个字段，用于指定请求或响应中携带的实体主体的媒体类型。
>
> 对于请求头，`Content-Type` 字段用于告诉服务器请求中携带的实体主体的媒体类型。常见的媒体类型包括 `application/json`、`application/x-www-form-urlencoded`、`multipart/form-data` 等。通过指定正确的 `Content-Type`，服务器可以正确地解析请求中的实体主体。
>
> 对于响应头，`Content-Type` 字段用于告诉客户端响应中携带的实体主体的媒体类型。客户端可以根据 `Content-Type` 来确定如何处理响应的实体主体，例如将其解析为 JSON、文本、图像等。

![image-20230801131536850](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801131536850.png)

尝试发送json数据：

![image-20230801132026247](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801132026247.png)

![image-20230801132045037](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801132045037.png)

得到要发送的关键字`name`

![image-20230801132141933](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801132141933.png)

然后是关键字`password`

![image-20230801135203249](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801135203249.png)

![image-20230801135155881](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801135155881.png)

同样的，在`Cookie`中加入数据`name, password`：

> Cookie 是由服务器发送给浏览器并存储在浏览器中的一小段文本数据，用于跟踪用户会话和存储用户相关信息。Cookie 的格式如下：
>
> ```
> Cookie: key1=value1; key2=value2; key3=value3
> ```
>
> 其中，`key1=value1`、`key2=value2`、`key3=value3` 是 Cookie 的键值对，多个键值对之间使用分号和空格进行分隔。

![image-20230801135556063](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801135556063.png)

![image-20230801135603344](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801135603344.png)

> `Basic `认证是一种 HTTP 认证机制，用于在请求头中发送用户名和密码进行身份验证。Basic 认证的格式如下：
>
> ```
> Authorization: Basic <base64(username:password)>
> ```
>
> 其中，`username` 是用户名，`password` 是密码。将 `username:password` 进行 Base64 编码，并将编码后的结果放在 `Authorization` 请求头中的 `Basic` 后面。

![image-20230801140130818](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801140130818.png)

得到flag

![image-20230801140150583](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\CNSS-Summer.assets\image-20230801140150583.png)

## CNSS Feedback Pre-alpha



## ezunserialize

![image-20230803094743878](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\practice\CNSS-Summer.assets\image-20230803094743878.png)

将源码复制到`vscode`，发现一个奇怪的编码，但是无所谓，我们只要利用好php本身自带的编码解码工具即可。

![image-20230803094849921](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\practice\CNSS-Summer.assets\image-20230803094849921.png)

运行程序，得到序列化后的URL编码：

```
O%3A4%3A%22CNSS%22%3A3%3A%7Bs%3A8%3A%22username%22%3Bs%3A5%3A%22admin%22%3Bs%3A11%3A%22%00%2A%00password%22%3Bs%3A3%3A%22ctf%22%3Bs%3A17%3A%22%00CNSS%00i_want2_say%22%3Bs%3A28%3A%22%E2%80%AE%E2%81%A6fssmsl%E2%81%A9%E2%81%A6i_like_web%22%3B%7D
```

注意，我们需要绕过`__wakeup()`方法，只需将类对象的个数改多就行，比如改成4，成功得到flag：

![image-20230803095023670](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\practice\CNSS-Summer.assets\image-20230803095023670.png)

# Reverse

## 回レ! 雪月花

![image-20230807180827141](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\practice\CNSS-Summer.assets\image-20230807180827141.png)

查看`cipher`：

![image-20230807180855866](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\practice\CNSS-Summer.assets\image-20230807180855866.png)

查看`encode`函数：

![image-20230807180952488](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\practice\CNSS-Summer.assets\image-20230807180952488.png)

发现flag进行了位运算处理，尝试编写脚本还原：

### flag提现

使用`x64dbg`找到程序运行部分

![image-20230811103458505](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\practice\CNSS-Summer.assets\image-20230811103458505.png)
