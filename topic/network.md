## 网络部分面试题

### 1. 从 URL 输入到页面展现发生了什么?
1. 在浏览器中输入url
2. 应用层DNS解析域名：先本地查找，再查询DNS服务器
3. 应用层客户端发送HTTP请求
4. 传输层TCP传输报文:三次握手
5. 网络层IP协议查询MAC地址
6. 数据到达数据链路层
7. 服务器接收数据
8. 服务器响应请求
9. 服务器返回相应文件
10. 页面渲染。解析HTML以构建DOM树 –> 构建渲染树 –> 布局渲染树 –> 绘制渲染树。

参考： [从输入URL到浏览器显示页面发生了什么](https://www.cnblogs.com/kongxy/p/4615226.html)

### 2.cookie和session的异同
1. cookie数据存放在客户的浏览器上，session数据放在服务器上。
2. cookie不是很安全，别人可以分析存放在本地的cookie并进行cookie欺骗，考虑到安全应当使用session。
3. session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能，考虑到减轻服务器性能方面，应当使用cookie。
4. 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie
5. 可以考虑将登陆信息等重要信息存放为session，其他信息如果需要保留，可以放在cookie中。

参考： [Session和Cookie的区别与联系](https://www.cnblogs.com/endlessdream/p/4699273.html)

### 3.说一下http和https的区别
+ https协议需要CA证书，费用较高。
+ http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
+ http协议的端口为80，https的端口为443。
+ http的连接是无状态的，https协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。
+ 提示：https的ssl加密是在 **传输层** 实现的。


### 4.ssl加密使用了哪种算法，如何加密
1. 在客户端与服务器间传输的数据是通过使用对称算法（如 DES 或 RC4）进行加密的。
2. 公用密钥算法（通常为 RSA）是用来获得加密密钥交换和数字签名的，此算法使用服务器的SSL数字证书中的公用密钥。

### 5.TCP三次握手的过程，为什么是三次而不是两次或者四次？
+ 第一次握手：客户端A发送一个syn（同步）包（syn=x）给服务器B，进入SYN_SEND状态，等待服务器确认
+ 第二次握手：服务端B收到客户端A发送的同步包，确认客户端的同步请求（ack=x+1）,同时也发送一个同步包，
也就是一个ACK包+SYN包服务器进入SYN_RECV状态
+ 第三次握手：客户端A收到服务器B的SYN+ACK包，向服务器B发送一个确认包，此包发送完毕，客户端和服务器进入
ESTABLISHED状态，完成三次握手
+ **三次握手的原因是为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误**

参考： [TCP 为什么是三次握手，而不是两次或四次?](https://www.zhihu.com/question/24853633)

### 6.TCP的四次挥手
第一次：主动关闭方发送一个FIN包，用来关闭主动关闭方到被动关闭方的数据传送，也就是告诉另一方我不再发送数据了，但此时仍可以接收数据

第二次：被动关闭方收到FIN包之后，发送一个确认（ACK）包给对方

第三次：被动关闭方发送一个FIN包，告诉对方不带发送数据

第四次：主动关闭方收到FIN包之后，发送一个ACK包给对方，至此完成四次挥手

### 7.HTTP报文的格式，传输中以何种方式传输
HTTP报文分为三个部分，起始行、首部和主体，其中起始行和首部以一个回车和换行符分隔，首部和主体以一个空行分隔，其中起始行是对这次HTTP请求或者响应的描述，请求报文的起始行包括使用的HTTP方法、请求的url地址、HTTP版本，响应报文的起始行包括HTTP的版本，HTTP状态码，http状态码的描述，首部也就是常说的HTTP头部，如Date、Cookie、Content-Type等，主体是这次请求或响应的数据，传输中以明文传输。

参考： [HTTP权威指南](https://book.douban.com/subject/10746113/)

### 8.常见的HTTP头部
可以将HTTP首部分为通用首部、请求首部、响应首部、实体首部，通用首部表示一些通用信息，如Date表示报文创建时间，请求首部就是请求报文中独有的，如cookie、和缓存相关的If-Modified-Since，响应首部就是响应报文中独有的，如set-cookie和重定向有关的location，实体首部用来描述实体部分，如Allow用来描述可执行的请求方法，Content-Type描述主体类型，Content-Encoding描述主体的编码方式

### 9.HTTP状态的简要分类
可以按照HTTP状态码的第一个数字分类，1xx表示信息，2xx表示成功，3xx表示重定向，这里需要注意的是304，表示未修改，4xx表示客户端错误，最常见的是404，5xx表示服务端错误。

### 10.HTTP状态码101、200、301、302、304的具体含义
101：切换协议 200：正常，OK，301：永久重定向，302：临时重定向，304：未修改

### 11.用户登陆过程的简要说明，如何判断用户是否登录？
用户输入用户名和密码，通过post请求将密码和用户名发送给服务器，服务器比对收到的用户名、密码和数据库中的数据进行比对，不一致则做出响应，反馈信息给客户端，如果比对一致则服务端生成一个session，这个session可以存储在内存、文件、数据库中，同时生成一个与之一一对应的sessionID作为cookie发送给客户端，比对成功之后反馈信息，这时一般会进行一次重定向，重定向至登陆之后的默认页面。判断用户登录则是根据这个sessionID，每次请求会先检查有没有这次类似sessionID的cookie发送过来，没有则认为没有登录，有则是否有相应的session，这个session是否过期等，来判断用户是否登录，登录是否过期。

### 12.tcp和udp的区别
+ TCP是面向连接的，UDP是无连接的即发送数据前不需要先建立连接。
+ TCP提供可靠的服务，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达。UDP尽最大努力交付，即不保证可靠交付。
+ UDP具有较好的实时性，工作效率比TCP高，适用于对高速传输和实时性有较高的通信或广播通信。
+ TCP连接只能是点到点、一对一的。UDP支持一对一，一对多，多对一和多对多的交互通信。
+ TCP的首部较大为20字节，而UDP只有8字节。

### 13.udp的阻塞机制，如何处理

### 14.简要介绍一下socket协议

### 15. Get和Post的区别

GET - 从指定的资源请求数据。 POST - 向指定的资源提交要被处理的数据。

然而，在以下情况中，请使用 POST 请求：

   无法使用缓存文件（更新服务器上的文件或数据库）

   向服务器发送大量数据（POST 没有数据量限制）

   发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

[GET对比POST](http://www.w3school.com.cn/tags/html_ref_httpmethods.asp)

### 16.什么是正向代理？什么是反向代理？
+ **正向代理** 就是客户端向代理服务器发送请求，并且指定目标服务器，之后代理向目标服务器转交并且将获得的内容返回给客户端。比如翻墙
+ **反向代理** 指代理会判断请求走向何处，并将请求转交给客户端，客户端只会觉得这个代理是一个真正的服务器。如负载均衡。

### 17.介绍一下HTTPS的连接过程
参考： [Https 建立安全连接的过程（SSL原理）](https://blog.csdn.net/xiaopang_yan/article/details/78709574)

### 18.介绍一下DNS的查找过程？
递归查询

第一步：在hosts静态文件、DNS解析器缓存中查找某主机的ip地址

第二步：上一步无法找到，去DNS本地服务器（即域服务器）查找，其本质是去区域服务器、服务器缓存中查找

第三步：本地DNS服务器查不到就根据‘根提示文件’向负责顶级域‘.com’的DNS服务器查询

第四步：‘根DNS服务器’根据查询域名中的‘xyz.com’，再向xyz.com的区域服务器查询

第五步：www.xyz.abc.com的DNS服务器直接解析该域名，将查询到的ip再原路返回给请求查询的主机

迭代查询参考：[DNS查询过程](https://www.cnblogs.com/vickey-wu/p/6557439.html)

### 19.http连接性能优化，长连接，keep-alive
HTTP1.1开始,默认采用持久连接,使用了一种叫做keepalive connections 的机制,它可以在传输数据后仍然保持连接,当客户端再次获取数据时,直接使用刚刚空闲下来的连接,而无需再次握手.低线路负载，提高传输速度.

Keep-Alive不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间。实现长连接需要客户端和服务端都支持长连接。

HTTP协议的长连接和短连接，实质上是TCP协议的长连接和短连接。

### 20. websocket 和http区别
1. websocket是持久连接的协议,而http是非持久连接的协议.
2. websocket是双向通信协议,模拟socket协议,可以双向发送消息,而http是单向的.
3. websocket的服务端可以主动向客服端发送信息,而http的服务端只有在客户端发起请求时才能发送数据,无法主动向客户端发送信息.

参考： [http,websocket和socket详解](https://blog.csdn.net/qq_38859786/article/details/80523642)

### 21.端口号的作用是什么？
作用是区分服务类别和同一时间进行多个会话

参考： [端口号的作用及常见端口号用途说明](https://www.2cto.com/net/201203/124616.html)

### 22. http 1.0 1.1 2.0的区别
+ **1.0和1.1的区别**
1. 缓存处理：HTTP1.1则引入了更多的缓存控制策略
2. 带宽优化：HTTP1.1则在请求头引入了range头域，它允许只请求资源的某个部分，即返回码是206。
3. 错误通知的管理：在HTTP1.1中新增了24个错误状态响应码
4. Host头处理：HTTP1.1的请求消息和响应消息都应支持Host头域，且请求消息中如果没有Host头域会报告一个错误
5. 长连接：HTTP 1.1支持长连接和请求的流水线处理，在一个TCP连接上可以传送多个HTTP请求和响应。
+ **1.x和2.0的区别**
1. **新的二进制格式：** HTTP1.x的解析是基于文本。2.0的协议解析决定采用二进制格式，实现方便且健壮。
2. **多路复用：** 即连接共享，即每一个request都是用作连接共享机制的。一个request对应一个id，这样一个连接上可以有多个request，每个连接的request可以随机的混杂在一起，接收方可以根据request的 id将request再归属到各自不同的服务端请求里面。
3. **header压缩：** HTTP2.0使用encoder来减少需要传输的header大小，HTTP2.0可以维护一个字典，差量更新HTTP头部，大大降低因头部传输产生的流量。
4. **服务端推送：** 服务端推送能把客户端所需要的资源伴随着index.html一起发送到客户端，省去了客户端重复请求的步骤。

参考： [HTTP1.0、HTTP1.1 和 HTTP2.0 的区别](https://mp.weixin.qq.com/s/GICbiyJpINrHZ41u_4zT-A)

### 23.post请求常见的content-type
1. **application/x-www-form-urlencoded**: 最常见的 POST 提交数据的方式了。浏览器的原生 form 表单，如果不设置 enctype 属性.
2. **multipart/form-data**: 使用表单上传文件时，必须让 form 的 enctyped 等于这个值。一般用来上传文件。
3. **application/json**：告诉服务端消息主体是序列化后的 JSON 字符串。上传复杂的结构化数据。
4. **text/xml**：使用 HTTP 作为传输协议，XML 作为编码方式的远程调用规范。

### 24.强制缓存和协商缓存
+ **强制缓存**： 浏览器在请求某一资源时，会先获取该资源缓存的header信息，判断是否命中强缓存（cache-control和expires信息），若命中直接从缓存中获取资源信息，包括缓存header信息；本次请求根本就不会与服务器进行通信；
+ **协商缓存**： 如果没有命中强缓存，浏览器会发送请求到服务器，请求会携带第一次请求返回的有关缓存的header字段信息（Last-Modified/If-Modified-Since和Etag/If-None-Match），由服务器根据请求中的相关header信息来比对结果是否协商缓存命中；若命中，则服务器返回新的响应header信息更新缓存中的对应header信息，但是并不返回资源内容，它会告知浏览器可以直接从缓存获取；否则返回最新的资源内容

参考： [http协商缓存VS强缓存](https://www.cnblogs.com/wonyun/p/5524617.html)

### 25.跨域有哪些方法
1. **JSONP**
2. **通过修改document.domain来跨子域。** 只能把document.domain设置成自身或更高一级的父域。
3. **使用window.name来进行跨域** ；window对象有个name属性，该属性有个特征：即在一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个window.name的，每个页面对window.name都有读写的权限，window.name是持久存在一个窗口载入过的所有页面中的，并不会因新页面的载入而进行重置。
4. **使用HTML5中新引进的window.postMessage方法来跨域传送数据** 使用它来向其它的window对象发送消息，无论这个window对象是属于同源或不同源，目前IE8+、FireFox、Chrome、Opera等浏览器都已经支持window.postMessage方法。
5. **CORS**
6. **服务器代理**
7. **flash**

### 26.写一个jsonp的实现
+ 利用了 **script** 标签没有跨域限制这一“漏洞”来达到与第三方通讯的目的。简单地说，该协议就是，允许用户传递一个callback参数给服务端，然后服务端返回数据时会将这个callback参数作为函数名包裹json数据，这样客户端就可以随意定制自己的函数自动处理返回的数据了。
```JavaScript
    var flightHandler = data=>{
      console.log(data);
    }
    var url = "http://flightQuery.com/jsonp/flightResult.aspx?code=CA1998&callback=flightHandler";
    var script = document.createElement('script');
    script.setAttribute('src', url);
    document.getElementsByTagName('head')[0].appendChild(script);
```

### 27.如何解决跨域问题
跨域解决方法： 1. CORS（跨域资源共享）； 2. JSONP跨域； 3. 图像ping跨域； 4. 服务器代理； 5. document.domain 跨域； 6. window.name 跨域； 7. location.hash 跨域； 8. postMessage 跨域；

参考： [浏览器同源策略及跨域的解决方法](https://www.cnblogs.com/laixiangran/p/9064769.html)

### 28.HTTP状态码
**200 OK**  请求已成功，请求所希望的响应头或数据体将随此响应返回。
**304 Not Modified** 服务器内容没有修改。
**307 Temporary Redirect** 重定向
**401 Unauthorized** 当前请求需要用户验证。
**404 Not Found** 请求失败。
**500 Internal Server Error** 服务器出错。