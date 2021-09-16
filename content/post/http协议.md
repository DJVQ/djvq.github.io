---
title: "HTTP协议"
date: 2021-06-20
tags: ["学习","HTTP","基础知识"]
description: "对于HTTP协议的一些认识"
---

## 简介
全称：Hyper Text Transfer Protocol（超文本传输协议）。

介绍：HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）、属于应用层的面向对象的协议、简捷、快速、适用于分布式超媒体信息系统。


## 主要特点
1. 简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
2. 灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。
3. 无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
4. 无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。
5. 支持B/S及C/S模式。

## URL
---
HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接。**URL**是一种特殊类型的URI，包含了用于查找某个资源的足够的信息。

---

URL,全称是Uniform Resource Locator, 中文叫统一资源定位符,是互联网上用来标识某一处资源的地址。

---
### URL的组成

1. 协议部分：该URL的协议部分为“http：”，这代表网页使用的是HTTP协议。在Internet中可以使用多种协议，如HTTP，FTP等等本例中使用的是HTTP协议。在"HTTP"后面的“//”为分隔符
2. 域名部分：例如“www.baidu.com”。一个URL中，也可以使用**IP地址**作为域名使用
3. 端口部分：跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。端口不是一个URL必须的部分，如果省略端口部分，将采用**默认端口**
4. 虚拟目录部分：从域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。虚拟目录也不是一个URL必须的部分。
5. 文件名部分：从域名后的最后一个“/”开始到“？”为止，是文件名部分，如果没有“?”,则是从域名后的最后一个“/”开始到“#”为止，是文件部分，如果没有“？”和“#”，那么从域名后的最后一个“/”开始到结束，都是文件名部分。例如“http://www.aspxfans.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name”，
文件名部分也不是一个URL必须的部分，如果省略该部分，则使用默认的文件名。
6. 锚部分：从“#”开始到最后，都是锚部分。本例中的锚部分是“name”。锚部分也不是一个URL必须的部分。锚点作用：顾名思义打开页面是滚动锚点位置。
7. 参数部分：从“？”开始到“#”为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为“boardID=5&ID=24618&page=1”。参数可以允许有多个参数，参数与参数之间用“&”作为分隔符。

### #（hash）的含义
---
hash 属性是一个可读可写的字符串，该字符串是 URL 的锚部分（从 # 号开始的部分）

---
1. #的涵义

#代表网页中的一个位置。其右面的字符，就是该位置的标识符。比如，
“http://www.example.com/index.html#print”，
就代表网页index.html的print位置。浏览器读取这个URL后，会自动将print位置滚动至可视区域。（单页应用）此外，为网页位置指定标识符，有两个方法。一是使用锚点，比如```<a name="print"></a>```，二是使用id属性，比如```<div id="print">```。

2. HTTP请求不包括#

#是用来指导浏览器动作的，对服务器端完全无用。所以，HTTP请求中不包括#。比如，访问下面的网址，“http://www.example.com/index.html#print”，
浏览器实际发出的请求是这样的：
```
　　GET /index.html HTTP/1.1
　　Host: www.example.com
```
可以看到，只是请求index.html，根本没有"#print"的部分。

3. #后的字符

在第一个#后面出现的任何字符，都会被浏览器解读为位置标识符。这意味着，这些字符都不会被发送到服务器端。

比如，下面URL的原意是指定一个颜色值：“http://www.example.com/?color=#fff”，
但是，浏览器实际发出的请求是：
```
　　GET /?color= HTTP/1.1
　　Host: www.example.com
```
可以看到，“#fff”被省略了。只有将#转码为%23，浏览器才会将其作为实义字符处理。也就是说，上面的网址应该被写成：“http://example.com/?color=%23fff”。

4. 改变#不触发网页重载

单单改变#后的部分，浏览器只会滚动到相应位置，不会重新加载网页。比如，从“http://www.example.com/index.html#location1”
改成“http://www.example.com/index.html#location2”
，浏览器不会重新向服务器请求index.html。

5. 改变#会改变浏览器的访问历史

每一次改变#后的部分，都会在浏览器的访问历史中增加一个记录，使用"后退"按钮，就可以回到上一个位置。这对于ajax应用程序特别有用，可以用不同的#值，表示不同的访问状态，然后向用户给出可以访问某个状态的链接。值得注意的是，上述规则对IE 6和IE 7不成立，它们不会因为#的改变而增加历史记录。

6. window.location.hash读取#值

window.location.hash这个属性可读可写。读取时，可以用来判断网页状态是否改变；写入时，则会在不重载网页的前提下，创造一条访问历史记录。

7. onhashchange事件

这是一个HTML 5新增的事件，当#值发生变化时，就会触发这个事件。IE8+、Firefox 3.6+、Chrome 5+、Safari 4.0+支持该事件。

它的使用方法有三种：
```
　　window.onhashchange = func;

　　<body οnhashchange="func();">

　　window.addEventListener("hashchange", func, false);
```
对于不支持onhashchange的浏览器，可以用setInterval监控location.hash的变化。

8. Google抓取#的机制

默认情况下，Google的网络蜘蛛忽视URL的#部分。但是，Google还规定，如果你希望Ajax生成的内容被浏览引擎读取，那么URL中可以使用"#!"，Google会自动将其后面的内容转成查询字符串_escaped_fragment_的值。比如，Google发现新版twitter的URL如下：

　　http://twitter.com/#!/username

就会自动抓取另一个URL：

　　http://twitter.com/?_escaped_fragment_=/username

通过这种机制，Google就可以索引动态的Ajax内容。

## URI和URL的区别

### URI，是uniform resource identifier，统一资源标识符，用来唯一的标识一个资源。

Web上可用的每种资源如HTML文档、图像、视频片段、程序等都是一个来URI来定位的
URI一般由三部组成：

① 访问资源的命名机制

② 存放资源的主机名

③ 资源自身的名称，由路径表示，着重强调于资源。

---
### URL是uniform resource locator，统一资源定位器，它是一种具体的URI，即URL可以用来标识一个资源，而且还指明了如何locate这个资源。

URL是Internet上用来描述信息资源的字符串，主要用在各种WWW客户程序和服务器程序上，特别是著名的Mosaic。

采用URL可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。URL一般由三部组成：

① 协议(或称为服务方式)

② 存有该资源的主机IP地址(有时也包括端口号)

③ 主机资源的具体地址。如目录和文件名等。

---
### URN，uniform resource name，统一资源命名，是通过名字来标识资源，比如mailto:java-net@java.sun.com。

URI是以一种抽象的，高层次概念定义统一资源标识，而URL和URN则是具体的资源标识的方式。URL和URN都是一种URI。笼统地说，每个 URL 都是 URI，但不一定每个 URI 都是 URL。这是因为 URI 还包括一个子类，即统一资源名称 (URN)，它命名资源但不指定如何定位资源。上面的 mailto、news 和 isbn URI 都是 URN 的示例。

在Java的URI中，一个URI实例可以代表绝对的，也可以是相对的，只要它符合URI的语法规则。而URL类则不仅符合语义，还包含了定位该资源的信息，因此它不能是相对的。在Java类库中，URI类不包含任何访问资源的方法，它唯一的作用就是解析。
相反的是，URL类可以打开一个到达资源的流。

## HTTP请求消息Request

客户端发送一个HTTP请求到服务器的请求消息包括以下格式：请求行（request line）、请求头部（header）、空行和请求数据四个部分组成。

![HTTP请求消息结构](https://upload-images.jianshu.io/upload_images/2964446-fdfb1a8fce8de946.png?imageMogr2/auto-orient/strip%7CimageView2/2)

POST请求例子，使用Fildder抓取的request：

```
POST https://api.live.bilibili.com/msg/send HTTP/1.1
Host: api.live.bilibili.com
Connection: keep-alive
Content-Length: 972
sec-ch-ua: " Not;A Brand";v="99", "Microsoft Edge";v="91", "Chromium";v="91"
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36 Edg/91.0.864.54
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary6h7ZSuakz2kYP6M1
Accept: */*
Origin: https://live.bilibili.com
Sec-Fetch-Site: same-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://live.bilibili.com/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Cookie: _uuid=*; buvid3=*; buvid_fp=*; DedeUserID=*; DedeUserID__ckMd5=*; CURRENT_FNVAL=*; blackside_state=*; rpdid=*; LIVE_BUVID=*; fingerprint3=*; fingerprint=*; fingerprint_s=*; SESSDATA=*; bili_jct=*; sid=*; buvid_fp_plain=*; bp_video_offset_270357795=*; CURRENT_QUALITY=*; Hm_lvt_8a6e55dbd2870f0f5bc9194cddf32a02=*,1624099878; bp_t_offset_270357795=*; _dfcaptcha=*; PVID=*3*

------WebKitFormBoundary6h7ZSuakz2kYP6M1
Content-Disposition: form-data; name="bubble"

0
------WebKitFormBoundary6h7ZSuakz2kYP6M1
Content-Disposition: form-data; name="msg"

打卡
------WebKitFormBoundary6h7ZSuakz2kYP6M1
Content-Disposition: form-data; name="color"

5566168
------WebKitFormBoundary6h7ZSuakz2kYP6M1
Content-Disposition: form-data; name="mode"

1
------WebKitFormBoundary6h7ZSuakz2kYP6M1
Content-Disposition: form-data; name="fontsize"

25
------WebKitFormBoundary6h7ZSuakz2kYP6M1
Content-Disposition: form-data; name="rnd"

1624179568
------WebKitFormBoundary6h7ZSuakz2kYP6M1
Content-Disposition: form-data; name="roomid"

80397
------WebKitFormBoundary6h7ZSuakz2kYP6M1
Content-Disposition: form-data; name="csrf"

586ac1b250f845350537e60b36f0bd9d
------WebKitFormBoundary6h7ZSuakz2kYP6M1
Content-Disposition: form-data; name="csrf_token"

586ac1b250f845350537e60b36f0bd9d
------WebKitFormBoundary6h7ZSuakz2kYP6M1--

```
上面是bilibili直播间发送“打卡”弹幕的request内容，第一行自然是请求行，第二行到第一个空行之前的内容自然是请求头，第一个空行也就是请求头后的空行是必须的，也就是上面提到的第三部分，第一个空行后面的内容为请求内容，也就是第四部分。

## HTTP之响应消息Response

一般情况下，服务器接收并处理客户端发过来的请求后会返回一个HTTP的响应消息。

HTTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文。

![](https://upload-images.jianshu.io/upload_images/2964446-1c4cab46f270d8ee.jpg?imageMogr2/auto-orient/strip%7CimageView2/2)

例子：
```
HTTP/1.1 200 OK
Date: Sun, 20 Jun 2021 08:59:47 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 42
Connection: keep-alive
bili-status-code: 0
bili-trace-id: bb4a4043450f35e1
Access-Control-Allow-Origin: https://live.bilibili.com
Access-Control-Allow-Credentials: true
Access-Control-Allow-Headers: Origin,No-Cache,X-Requested-With,If-Modified-Since,Pragma,Last-Modified,Cache-Control,Expires,Content-Type,Access-Control-Allow-Credentials,DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Cache-Webcdn,x-bilibili-key-real-ip,x-backend-bili-real-ip
Vary: Accept-Encoding,Origin
Expires: Sun, 20 Jun 2021 08:59:46 GMT
Cache-Control: no-cache
X-Cache-Webcdn: BYPASS from hw-sh3-webcdn-04

{"code":0,"data":[],"message":"","msg":""}
```
上面是前面发送弹幕请求的响应，第一行自然是状态行；第一行到第一个空行前的内容自然是消息报头；第一个空行自然是必须的；第一个空行后面的内容自然是响应正文。

## HTTP状态码

状态代码有三位数字组成，第一个数字定义了响应的类别，共分五种类别:

1xx：指示信息--表示请求已接收，继续处理

2xx：成功--表示请求已被成功接收、理解、接受

3xx：重定向--要完成请求必须进行更进一步的操作

4xx：客户端错误--请求有语法错误或请求无法实现

5xx：服务器端错误--服务器未能实现合法的请求

常见的状态码：
```
200 OK                        //客户端请求成功
400 Bad Request               //客户端请求有语法错误，不能被服务器所理解
401 Unauthorized              //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用 
403 Forbidden                 //服务器收到请求，但是拒绝提供服务
404 Not Found                 //请求资源不存在，eg：输入了错误的URL
500 Internal Server Error     //服务器发生不可预期的错误
503 Server Unavailable        //服务器当前不能处理客户端的请求，一段时间后可能恢复正常
```

## HTTP请求方法

根据HTTP标准，HTTP请求可以使用多种请求方法。

HTTP1.0定义了三种请求方法： GET, POST 和 HEAD方法。

HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法。
```
GET     请求指定的页面信息，并返回实体主体。
HEAD     类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
POST     向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
PUT     从客户端向服务器传送的数据取代指定的文档的内容。
DELETE      请求服务器删除指定的页面。
CONNECT     HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
OPTIONS     允许客户端查看服务器的性能。
TRACE     回显服务器收到的请求，主要用于测试或诊断。
```

## HTTP工作原理

HTTP协议定义Web客户端如何从Web服务器请求Web页面，以及服务器如何把Web页面传送给客户端。HTTP协议采用了请求/响应模型。客户端向服务器发送一个请求报文，请求报文包含请求的方法、URL、协议版本、请求头部和请求数据。服务器以一个状态行作为响应，响应的内容包括协议的版本、成功或者错误代码、服务器信息、响应头部和响应数据。

以下是 HTTP 请求/响应的步骤：
1. 客户端连接到Web服务器

一个HTTP客户端，通常是浏览器，与Web服务器的HTTP端口（默认为80）建立一个TCP套接字连接。例如，http://www.baidu.com。

2. 发送HTTP请求

通过TCP套接字，客户端向Web服务器发送一个文本的请求报文，一个请求报文由请求行、请求头部、空行和请求数据4部分组成。

3. 服务器接受请求并返回HTTP响应

Web服务器解析请求，定位请求资源。服务器将资源复本写到TCP套接字，由客户端读取。一个响应由状态行、响应头部、空行和响应数据4部分组成。

4. 释放连接TCP连接

若connection 模式为close，则服务器主动关闭TCP连接，客户端被动关闭连接，释放TCP连接;若connection 模式为keepalive，则该连接会保持一段时间，在该时间内可以继续接收请求。

5. 客户端浏览器解析HTML内容

客户端浏览器首先解析状态行，查看表明请求是否成功的状态代码。然后解析每一个响应头，响应头告知以下为若干字节的HTML文档和文档的字符集。客户端浏览器读取响应数据HTML，根据HTML的语法对其进行格式化，并在浏览器窗口中显示。

例如：在浏览器地址栏键入URL，按下回车之后会经历以下流程：

1、浏览器向 DNS 服务器请求解析该 URL 中的域名所对应的 IP 地址;

2、解析出 IP 地址后，根据该 IP 地址和默认端口 80，和服务器建立TCP连接;

3、浏览器发出读取文件(URL 中域名后面部分对应的文件)的HTTP 请求，该请求报文作为 TCP 三次握手的第三个报文的数据发送给服务器;

4、服务器对浏览器请求作出响应，并把对应的 html 文本发送给浏览器;

5、释放 TCP连接;

6、浏览器将该 html 文本并显示内容; 　

## GET和POST请求的区别
```
GET请求
GET /books/?sex=man&name=Professional HTTP/1.1
Host: www.wrox.com
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
Gecko/20050225 Firefox/1.0.1
Connection: Keep-Alive
注意最后一行是空行

POST请求
POST / HTTP/1.1
Host: www.wrox.com
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
Gecko/20050225 Firefox/1.0.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 40
Connection: Keep-Alive

name=Professional%20Ajax&publisher=Wiley
```
---
### 提交数据位置
GET提交，请求的数据会附在URL之后（就是把数据放置在HTTP协议头中），以?分割URL和传输数据，多个参数用&连接；例 如：login.action?name=hyddd&password=idontknow&verify=%E4%BD%A0 %E5%A5%BD。如果数据是英文字母/数字，原样发送，如果是空格，转换为+，如果是中文/其他字符，则直接把字符串用BASE64加密，得出如： %E4%BD%A0%E5%A5%BD，其中％XX中的XX为该符号以16进制表示的ASCII。

POST提交：把提交的数据放置在是HTTP包的包体中。

**因此，GET提交的数据会在地址栏中显示出来，而POST提交，地址栏不会改变。**

---
### 传输数据大小
传输数据的大小：首先声明：HTTP协议没有对传输的数据大小进行限制，HTTP协议规范也没有对URL长度进行限制。而在实际开发中存在的限制主要有：

GET:特定浏览器和服务器对URL长度有限制，例如 IE对URL长度的限制是2083字节(2K+35)。对于其他浏览器，如Netscape、FireFox等，理论上没有长度限制，其限制取决于操作系统的支持。

因此对于GET提交时，传输数据就会受到URL长度的 限制。

POST:由于不是通过URL传值，理论上数据不受限。但实际各个WEB服务器会规定对post提交数据大小进行限制，Apache、IIS6都有各自的配置。

---
### 安全性

POST的安全性要比GET的安全性高。比如：通过GET提交数据，用户名和密码将明文出现在URL上，因为(1)登录页面有可能被浏览器缓存；(2)其他人查看浏览器的历史纪录，那么别人就可以拿到你的账号和密码了，除此之外，使用GET提交数据还可能会造成Cross-site request forgery攻击

---
### Http get,post,soap协议都是在http上运行的

1. get：请求参数是作为一个key/value对的序列（查询字符串）附加到URL上的查询字符串的长度受到web浏览器和web服务器的限制（如IE最多支持2048个字符），不适合传输大型数据集同时，它很不安全

2. post：请求参数是在http标题的一个不同部分（名为entity body）传输的，这一部分用来传输表单信息，因此必须将Content-type设置为:application/x-www-form- urlencoded。post设计用来支持web窗体上的用户字段，其参数也是作为key/value对传输。但是：它不支持复杂数据类型，因为post没有定义传输数据结构的语义和规则。

3. soap：是http post的一个专用版本，遵循一种特殊的xml消息格式，Content-type设置为: text/xml 任何数据都可以xml化。

Http协议定义了很多与服务器交互的方法，最基本的有4种，分别是GET,POST,PUT,DELETE. 一个URL地址用于描述一个网络上的资源，而HTTP中的GET, POST, PUT, DELETE就对应着对这个资源的查，改，增，删4个操作。 我们最常见的就是GET和POST了。GET一般用于获取/查询资源信息，而POST一般用于更新资源信息.

---
### GET和POST的区别

1. GET提交的数据会放在URL之后，以?分割URL和传输数据，参数之间以&相连，如EditPosts.aspx?name=test1&id=123456. POST方法是把提交的数据放在HTTP包的Body中.

2. GET提交的数据大小有限制（因为浏览器对URL的长度有限制），而POST方法提交的数据没有限制.

3. GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值。

4. GET方式提交数据，会带来安全问题，比如一个登录页面，通过GET方式提交数据时，用户名和密码将出现在URL上，如果页面可以被缓存或者其他人可以访问这台机器，就可以从历史记录获得该用户的账号和密码.