# HTTP/HTTPS

## HTTP概述

> 1989年，蒂姆·伯纳斯 - 李（Tim Berners-Lee）在论文中提出可以在互联网上构建超链接文档，并提出了三点.

URI：统一资源标识符。互联网的唯一ID

HTML：超文本文档

HTTP：传输超文本的文本传输协议

### HTTP是什么

​		`HTTP(hypertext transport protocol)`翻译过来为"超文本传输协议"，文本可以理解为简单的字符文字组合，也可以理解为更为复杂的音频或者图像等。那么将这个词语拆分为三个部分。

<img src="https://mmbiz.qpic.cn/mmbiz_png/NdsdouZwicacugPgPuxCgLUTf9ErM0xpa7wLExBeOPwYvyea0PDosCAMibbmSJkxcQGUjLYlEQGIiagdSxC0673RQ/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />

​		"超文本"和"文本"相比多了一个字"超"，这样看来比文本丰富，因为它可以将多种文本/图像等进行混合，更重要的是可以从一个文本跳转到另一个文本(文本连接)。

​		"传输"，传输的过程中需要沟通，沟通即可能一对一沟通也可能一对多沟通(进行内容协商)，无论怎么样，参加沟通的人数>1，想尽一切一切办法更快更好的完成相应的任务。

​		"协议"，无规矩不成方圆，做机密项目之前需要签署保密协议，找工作要签"三方协议"，三方协议是学校，公司，和个人组成的协议，都是为了让大家受一定的约束，违反了即有相应的惩罚。

​		HTTP是一种无状态协议。协议自身不对请求和响应之间的通信状态进行保存，即HTTP这个级别，协议对于发送过的请求或响应都不做持久化处理。

### HTTP应用

Web服务器、浏览器、CDN、爬虫、WAF

## 与HTTP相关协议

与HTTP关系密切的协议包括：IP、TCP、DNS



## HTTP版本

### HTTP/0.9

​		0.9版本相对简单，采用纯文本格式，且设置为只读，所以当时只能使用"Get"的方式从服务器获得HTML文档，响应以后则关闭。

```http
GET /Mysite.html
```

响应中只包含了文档本身。响应内容无响应头，无错误码，无状态码，可以说是"裸奔"。

```http
<HTML>
Hello world
</HTML>
HTTP/1.0
```

此时HTTP/0.9请求过程如下

- 应用层的HTTP建立在传输层的TCP之上并运用TCP可靠性等特性，先三次握手建立连接
- 客户端请求建立连接(此时只有GET)
- 服务端响应请求，数据以 ASCII 字符流返回给客戶端
- 传输完成，断开连接。

<img src="https://mmbiz.qpic.cn/mmbiz_png/NdsdouZwicacugPgPuxCgLUTf9ErM0xpah4SCcBx6dKhXSccJaR4Umjxoz3FSjKnn3ZRhUibHviaWcgqx9rDpZSVA/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />

### HTTP/1.0

​		随着时代的进步，仅仅文本的传输无法满足需求，更多情况需要采用图文的方式才能生动的表达出自己的观点。随着1995年开发出Apache，同时其他的多媒体等技术发展迅速，从而进一步的促使HTTP新功能的出现。HTTP1.0在1996年诞生，增加了一下几个方面：

- 增加Post(加参数)，Head方法
- 加入协议版本号，同时添加文件处理类型
- 加入HTTP Header，让HTTP处理请求更加灵活
- 增加响应状态码，标记出错的原因
- 提供国际化(不同语言)支持

<img src="https://mmbiz.qpic.cn/mmbiz_png/NdsdouZwicacugPgPuxCgLUTf9ErM0xpa9mgpSKUWSQiaGicCySuYzdWnJvCky55tkWibKictmt8f93MCm3FvTmcUOA/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />

### HTTP/1.1

1999年HTTP/1.1发布并成为标准，写入RFC，以为以后不管是网关还是APP等，只要你要使用HTTP，就得遵守这个标准。

- 继续增加了PUT等方法
- 允许持久连接
- 强制要求Host头

随着文件越来越大，图片等信息越来越复杂，如果每一次上传下载文件都需要建立连接断开连接的过程将增加大量的开销。为此，提出了持久连接，也就是一次TCP连接可以具有多个HTTP请求。当然持久连接是可选择的，如果考虑关闭，只需要使用Connecttion:close关闭即可。

<img src="https://mmbiz.qpic.cn/mmbiz_png/NdsdouZwicacugPgPuxCgLUTf9ErM0xpaIWI2ricDYKO0D7DGOhpgC3IZKvMF4jFzib1QbyIcUQhqOtkyMbf00sUQ/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom: 50%;" />

​		在电商系统中，经常会因为促销活动导致流量飙升，为了缓解流量，其中有种方法即加缓存或者加服务器。如果是单台服务器负载过大，数据库可能分库分表。数据结构算法中分而治之方法亦是如此。那么HTTP中，同样的道理，如果文件太大，就大文件切分为小文件块发送。

### HTTP/2

---多路复用

HTTP/1.1的出现，几年间出来大量牛掰的互联网公司，发展实在是太快，但是HTTP1.1中这几点成为诟病

- 原因1 TCP自带慢启动

顾名思义，"慢启动"从0到1循循渐进。轿车启动不会按下按钮就直接起飞，而是缓慢调节到适合的速度。这不是挺好的？为什么会带来性能问题呢。我们知道一个页面有静态数据，动态页面，很多小文件在加载的过程中就会直接发起请求，这样导致太多的请求都会经历慢启动过程，花费时间太多。

- 原因2 多条TCP连接带宽竞争

带宽固定，多条TCP连接同时发起竞争带宽资源，由于各个TCP连接之间没有通信机制，也无法得知哪些资源优先级更高，从而导致想快速下载的资源反而延迟下载。

- 原因3 头部阻塞

阻塞，在网络编程中，我们采用异步，多路复用(epoll)方式尽量让cpu少等待多干事。在HTTP1.1中，虽然大家共用了一条TCP通道，但是第一个请求没有结束，第二请求就可能阻塞等待，也就是说不能同时发送接收数据。那么一个网页很多数据文件，如果能够同时发出请求，让部分数据文件能够得到响应并预处理，这样就大大的利用了带宽和cpu的资源。基于这些因素，在HTTP2中出现了新的方案

***如何解决头部阻塞呢？***

HTTP是一问一答的模式，大家都在这个队列排队导致堵塞，那就多个队列并发进行，也就是"对同一个域名发起多个长连接"。举个例子，在火车站排队买票的时候，如果只有一个窗口可用，大家只能苦等，多开几个窗口就可缓解这个问题。

这个时候用户数 * 并发数(上限6-8)已经不错得效果，但是互联网速度太快，火车站就这么大，窗口也就这么多，怎么办，建新的火车站进行分流(大部分城市都有什么东站 西站)。在这里叫做"域名分片"，使用多个域名，这些域名指向同一服务器。

### HTTP/3

HTTP/2看似很完美了吧，但是Google轮子哥可不服，其他人在研究HTTP/2的时候，它们就在琢磨QUIC。那QUIC有啥牛掰的地方呢

> QUIC是Google开发的一个基于UDP且能像TCP一样具有可靠性特点的协议。具备像HTTP/2一样的应用数据二进制分帧传输。其主要解决的问题有两个。

1. 进一步解决线头阻塞问题。通过独立不同流，让各个流之间实现相互独立传输，互不干扰
2. 切换网络时的连接保持。wifi和3G/4G经常需要来回切换。基于TCP的协议，会因为网络的切换导致IP地址的改变。而基于UDP的QUIC协议，及时切换也可以恢复之前与服务器的连接。(这里推荐大家可以去看看MPTCP)

## HTTP报文详解

> 客户端与服务端进行交互的信息为报文。客户端为请求报文，服务端为响应报文。

![报文层次结构](https://mmbiz.qpic.cn/mmbiz_png/NdsdouZwicacugPgPuxCgLUTf9ErM0xpazVI6vNzfoEwSqZuxvUjQW3EWJxkQ3KYcPw11Aic4O0Ngd6AN6QrZ4WQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```http
GET /article/12 HTTP/1.1
Host: www.xxx.cn
Connection: keep-alive
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.106 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: SESSION=so9nlsvenminor5abs65sh9dsa
```

```HTTP
HTTP/1.1 200 OK
Server: nginx
Date: Sun, 17 May 2020 17:04:29 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
X-Powered-By: blade-2.0.6-BETA
Content-Encoding: gzip
```

### HTTP报文首部

![image-20200614154252643](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200614154252643.png)

​		HTTP协议的请求和响应报文中必定包含HTTP首部。首部内容为客户端和服务器分别处理请求和响应提供所需要的信息。对于客户端用户来说，这些信息中的大部分内容都无需亲自查看。

#### 请求报文

<img src="https://mmbiz.qpic.cn/mmbiz_png/NdsdouZwicacugPgPuxCgLUTf9ErM0xpaF9KxT9AicNZL1qznvK2UqrcgIRkH30LZsXmico88QVBMsZFtOjLlh8ZA/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom: 50%;" />

![image-20200614160021364](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200614160021364.png)

请求报文通常由三部分组成：

**起始行**：描述请求或者响应的基本信息

**首部字段**：形式说明报文。包含表示请求的各种条件和属性的各类首部。

​				一般有四种首部，分别是：通用首部、请求首部、响应首部、实体首部。

**消息正文**：实际传输诸如图片等信息。具体如下图试试

##### **请求方法**

一共有八种方法选择，如下图所示。采用不同的方法获取不同的资源。

<img src="https://mmbiz.qpic.cn/mmbiz_png/NdsdouZwicacugPgPuxCgLUTf9ErM0xpahliaibFI2zGBroGCks7JC6pK5iavNWbdMawVwKia1JiamobC6T5BnShTwibw/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom: 80%;" />

常见的请求方法：

- `GET`：获取资源，从服务器中取资源。向特定的资源发出请求，可以请求图片，视频等。指定的资源经服务器端解析后返回响应内容。如果请求的是程序，则返回经执行后的输出结果。
- `POST`：传输实体主体。POST功能与GET很相似，但POST的主要目的不是获取响应的主体内容。向指定资源提交数据进行处理请求（例如提交表单或者上传文件） 。数据被包含在请求体中。POST 请求可能会导致新的资源的建立或对已有资源的修改。
- `PUT`：传输文件（客户端向服务器）。要求在请求报文的主体中包含文件内容，然后保存到URI指定的位置。向指定资源位置上传其最新内容。
- `HEAD`： 获得报文首部。和GET类似，只是不返回报文主体部分。用于确认URI的有效性及资源更新的日期时间等。该方法常用于测试超链接的有效性，是否可以访问，以及最近是否更新等信息。
- `TRACE`：回显服务器收到的请求，主要用于测试或诊断。
- `OPTIONS`：返回服务器针对特定资源所支持的HTTP 请求方法

##### 常见请求头

在HTT P/ 1.1 协议中，所有的请求头（除Host 外）都是可选的。

`Host` ：（发送请求时，该请求头是必需的）主要用于指定被请求资源的Internet 主机和端口号，它通常从HTTP URL 中提取出来的。

`Connection` ：它的值通常只有两个， keep-alive 和close 。

`Accept` ：浏览器端可以接收的MIME 类型。例Accept: text/html 代表浏览器可以接收服务器回发的类型为text/html ，也就是html 文档。如果服务器无法返回text/html 类型的数据，服务器应该返回一个406 错误（ non acceptable ） 。

`Cache-Control` ：指定请求和响应遵循的缓存机制。

`Accept-Encoding` ：浏览器声明自己可接收的编码方法，通常说明是否支持压缩并指定压缩方法； Servlet 能够向支持gzip 的浏览器返回经gzip 编码的HTML 页面。

`Accept-Language` ：浏览器声明自己接收的语言。

`User-Agent`：用于告诉HTTP 服务器，客户端使用的操作系统和浏览器的名称和版本。

`Cookie` ：最重要的请求头之一，用于将cookie 的值发送给HTTP 服务器。

`Content-Length`：表示请求消息正文的长度。

**GET和POST区别**

GET一般用于获取/查询资源信息，而POST一般用于更新资源信息

- 根据HTTP规范，GET用于信息获取，而且应该是**安全**的和**幂等**的。 (1).所谓安全的意味着该操作用于获取信息而非修改信息。换句话说，GET 请求一般不应产生副作用。就是说，它仅仅是获取资源信息，就像数据库查询一样，不会修改、增加数据，不会影响资源的状态。 注意：这里安全的含义仅仅是指是非修改信息。 (2).幂等的意味着对同一URL的多个请求应该返回同样的结果。
- 根据HTTP规范，POST表示可能修改变服务器上的资源的请求。
- GET提交数据会有大小的限制（1024k），POST提交无限制

#### 响应报文

<img src="https://mmbiz.qpic.cn/mmbiz_png/NdsdouZwicacugPgPuxCgLUTf9ErM0xpa94tBejDFIKX79SibLfYxT0vfFXYMvgs3QeXB0HFVTz5FTIDqUhkRsHQ/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />

![image-20200614161915707](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200614161915707.png)

<1> 版本号：使用的HTTP什么版本

<2> 状态码：不同数字代表不同的结果，就如我们在编码时，通过返回不同的值代表不同的语义。

##### 状态码

状态码一共分为5类。

|      | 类别                             | 原因短语                   |
| ---- | -------------------------------- | -------------------------- |
| 1xx  | Informational（信息性状态码）    | 接收的请求正在处理         |
| 2xx  | Success（成功状态码）            | 请求正常处理完毕           |
| 3xx  | Redirection（重定向状态码）      | 需要进行附加操作以完成请求 |
| 4xx  | Client Error（客户端错误状态码） | 服务器无法处理请求         |
| 5xx  | Server Error（服务器错误状态码） | 服务器处理请求出错         |

> 1××：处于中间状态，还需后续操作
>
> 2××：成功收到报文并正确处理

`"200 OK"` 最常见的成功状态码，表示一切正常，客户端获得期许的处理结果。如果不是Head请求，那么在响应头中通常会有body数据

`"204 No Content"` 这个的含义和"200"很相似，不同之处在于它的响应头中没有body数据。

`"206 Partial Content"` 是 HTTP 分块下载或断点续传的基础，在客户端发送“范围请求”、要求获取资源的部分数据时出现，它与 200 一样，也是服务器成功处理了请求，但 body 里的数据不是资源的全部，而是其中的一部分。状态码 206 通常还会伴随着头字段“Content-Range”，表示响应报文里 body 数据的具体范围，供客户端确认，例如“Content-Range: bytes 0-99/5000”，意思是此次获取的是总计 5000 个字节的前 100 个字节。

> 3××：重定向到其他资源位置

`"301 Moved Permanently"` “永久重定向”，意思是本地请求的资源以及不存在，使用新的URI再次访问。

> 4××：请求报文有误，服务器无法处理

`"400 Bad Request”` 通用错误码，表示请求报文有错误，但是这个错误过于笼统。不知道是客户端还是哪里的错误，所以在实际应用中，通常会返回含有明确含义的状态码。

> 5××：服务器错误，服务器对请求出的时候发生内部错误。

`“500 Internal Server Error”` 和400 类似，属于一个通用的错误码，但是服务器到底是什么错误我们不得而知。其实这是好事，尽量少的将服务器资源暴露外网，尽量保证服务器的安全。

**常见状态代码、状态描述：**

- 200 OK：客户端请求成功
- 204 No Content：没有新文档，浏览器应该继续显示原来的文档
- 206 Partial Content：客户发送了一个带有Range头的GET请求，服务器完成了它
- 301 Moved Permanently：所请求的页面已经转移至新的url
- 302 Found：所请求的页面已经临时转移至新的url
- 304 Not Modified：客户端有缓冲的文档并发出了一个条件性的请求，服务器告诉客户，原来缓冲的文档还可以继续使用。
- 400 Bad Request：客户端请求有语法错误，不能被服务器所理解
- 401 Unauthorized：请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
- 403 Forbidden：对被请求页面的访问被禁止
- 404 Not Found：请求资源不存在
- 500 Internal Server Error：服务器发生不可预期的错误
- 503 Server Unavailable：请求未完成，服务器临时过载或当机，一段时间后可能恢复正常

### HTTP首部字段

HTTP首部字段是构成HTTP报文的要素之一。在客户端与服务器之间以HTTP协议进行通信的过程中，无论是请求还是响应都会使用首部字段，能起到传递额外重要信息的作用。

使用首部字段是为了给浏览器和服务器提供报文主体大小、所使用的语言、认证信息等内容。

**HTTP首部字段结构**

HTTP首部字段是由首部字段名和字段值构成，中间用冒号“：”分隔，多个字段值用“，”分隔。

```http
首部字段名：字段值
```

例：在HTTP首部中以`Content-Type`这个字段表示报文主体的对象类型。

```HTTP
Content-Type: text/htm;
```

**通用首部字段`General Header Fields`**

请求报文和响应报文两方都会使用的首部。

**请求首部字段`Request Head Fields`**

从客户端向服务器端发送请求报文时使用的首部。补充了请求的附加内容、客户端信息、响应内容相关优先级等信息。

**响应首部字段`Response Header Fields`**

从服务器端向客户端返回响应报文时使用的首部。补充了响应的附加内容，也会要求客户端附加额外的内容信息。

**实体首部字段`Entity Header Fields`**

针对请求报文和响应报文的实体部分使用的首部。补充了资源内容更新时间等于实体有关的信息。

### 请求体

> 上面大部分都是涉及到header部分，还有非常重要的body，everybody

头字段注意事项

HTTP的body常常被分为这几种的类别

<1> text:超文本text/html,纯文本text/plain

<2> audio/video:音视频数据

<3> application: 可能是文本，也可能是二进制，交给上层应用处理

<4> image: 图像文件。image/png等

#### 数据压缩

带宽一定，数据大了通常考虑使用压缩算法进行压缩，在HTTP中使用Encoding type表示，常用的压缩方式有下面几种

<1> gzip:

> 一种数据格式，默认且目前仅使用deflate算法压缩data部分

<2> deflate:

> deflate是一种压缩算法，是huffman编码的一种加强

<3> br:

> br通过变种的LZ77算法、Huffman编码以及二阶文本建模等方式进行数据压缩，其他压缩算法相比，它有着更高的压塑压缩效率

## URI

> 统一资源标识符(Uniform Resource Identifier),严格来说不等于网址，它包含URL和URN，可是URL太出名了以致于URL="网址"。无论开发，测试运维配置都离不开URI,所以好好掌握。

​		网络层的IP主要目的是解决路由和寻址。现在的IP地址按照"."分割，总共2的32次方大约42亿。对于计算机来说比较方便，但是对于人类来说还是不容易记忆，此时出现DNS了，他把IP地址映射为我们平时常见的"redis.org"，按照"."分割域名，从左到右级别越高，最右边为"顶级域名"。

<img src="https://mmbiz.qpic.cn/mmbiz_png/NdsdouZwicacugPgPuxCgLUTf9ErM0xpao2ibLHWY8rTnULDAFgtibF4SeXQMAWgaDP3ztQT2J5LUEhWUp8zIr4VQ/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom: 60%;" />

​		现在TCP提供可靠(数据不丢失)且字节流(数据完整性)，而且也有方便我们记忆的域名，但是互联网资源千万种，也不知道访问什么(图片，文字，视频一大堆)，这个时候URI(统一资源标识符)出现了，那长啥样？

<img src="https://mmbiz.qpic.cn/mmbiz_png/NdsdouZwicacugPgPuxCgLUTf9ErM0xpaiaCFC1q7KEl4coGPaYnzROiakib0oRNZfAgjf3Viax6OQiaicWyibgkeSHYnQ/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom: 67%;" />

![image-20200613193243744](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613193243744.png)

**协议名**：HTTP协议，另外还有ftp等协议。告知访问资源时使用什么协议。

紧接着是分隔符:"://"

**登录认证信息**：登录主机时的用户名密码(不建议，直接告诉了别人你的隐私信息)

**服务器地址：**使用绝对URI必须指定待访问的服务器地址。可以是DNS可解析的地址，也可以是IPv4/IPv6地址。

**服务器端口号：**指定服务器连接的网络端口号。

**层次文件路径**：资源在主机上的位置，使用“/”分隔多级目录，在这里是“/en/download.html”。注意，必须"/"开头

**查询字符串**：可选项。用"?"开始，表示额外的请求要求。通常使用"key=value"的方式存在，如果多个"key=value"则使用"&"相连。

**片段标识符：**可选项。标记已获取的子资源（文档内的某个位置）。	

看几个例子

http://nginx.org/en/download.html

file:///E:/Demo/index/

> 这里注意是三个"///"，因为前面"://"作为分隔符，资源路径按照"/"开头。

​		既然规则这么多，对于接收方而言需要完成的解析也需要遵守规则，全球用户很多使用HTTP，每个国家地区所使用语言不同，HTTP为了能对其进行统一处理，引入了URI编码，方法比较简单，将非ASCII或者特殊字符全部转换为十六进制字节值，同时在前面加入"%"。比如空格被转换为"%20","中国"就编码为"%E4%B8%AD%E5%9B%BD%0A"。

## HTTP中的一些技术

### 分块传输

​		使用相应的压缩方法在带宽一定的情况下确实有不错的效果，但是gzip等主要针对文件压缩效果不错，但是对视频就不行了。这个时候是不是可以使用数据结构中常用的分而治之，大化小再合并的方式呢。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613152148852.png" alt="image-20200613152148852" style="zoom:50%;" />

​		在报文中使用"Transer-Encoding:chunked"表示，代表body部分数据是分块传输的。另外在body中存在一个content-length字段表示body的长度，两者不能共存，另外很多时候是流式数据，body中没有指明content-length，这个时候一般就是chunked传输了。

现在可以通过采用分块的方式增强带宽的利用率，那他的编码规则如何呢

<1> 每一个分块包含长度和数据块

<2> 长度头按照CRLF结束

<3> 数据块在长度快后，且最后CRLF结尾

<4> 使用长度0表示结束，"0\r\n\r\n"

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613152243207.png" alt="image-20200613152243207" style="zoom: 50%;" />

### 范围传输		

分块解决了一部分问题，但是有的时候我们想截断发送怎么办呢。在HTTP中提供了使用字段“Accept - Ranges: bytes”，明确告知客户端：“我是支持范围请求的”。那么Range范围是怎样的呢，Range从0开始计算，比如Range:0-5则读取前6个字节，服务器收到了这个请求，将如何回应呢

<1> 合法性检查。比如一共只有20字节，但是请求range：100-200。此时会返回416----"范围请求有误"

<2> 范围正常，则返回216，表示请求数据知识一部分

<3> 服务器端在相应投资端增加Content-Range,格式"bytes x-y/length"。

### 断点续传

<1> 查看服务器是否支持范围请求并记录文件大小

<2> 多个线程分别负责不同的range

<3> 下载同时记录进度，即使因为网络等原因中断也没事，Range请求剩余即可

### Cookie机制

> HTTP是无状态、无记忆的，Cookie机制的出现让其有记忆功能，是怎么个实现呢

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613104438303.png" alt="image-20200613104438303" style="zoom: 50%;" />

Cookie是由浏览器负责存储，并不是操作系统负责，我们换个浏览器打开同样的网页，服务器就认不出来了。

Cookie常见的应用一个是身份识别，一个是广告追踪，比如我们在访问网页视频或者图片的时候，广告商会悄悄给我们Cookie打上标记，方、便做关联分析和行为分析，从而给我推荐一些相关内容。

### 通信数据转发程序：代理、网关、隧道

​		HTTP通信时除了客户端和服务器外，还有一些用于通信数据转发的应用程序，配合服务器工作。这些应用程序和服务器可以将请求转发给通信线路上的下一站服务器，并且能接收从那台服务器发送的响应再转发给客户端。

​		常见的应用程序包括代理、网关、隧道。

#### HTTP代理

> 之前介绍的都是一问一答的情景，但是在大部分的情况下都会存在多台服务器进行通信服务。其中比较常见的就是在请求方与应答方中间增加一个中间代理。它扮演了“中间人”的角色。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613111109420.png" alt="image-20200613111109420" style="zoom:50%;" />

​		代理作为中间位置，相对请求方为服务端，相当于后端服务端为请求方。代理常见的功能为负载均衡。在负载均衡中需要区分正向代理与反向代理，其中也就会涉及调度算法，比如轮询，一致性哈希等。

​		在HTTP通信过程中，可以级联多台代理服务器。请求和响应的转发会经过数台类似锁链一样连接起来的代理服务器。转发时，需要附加Via首部字段以标记处经过的主机信息。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613120312835.png" alt="image-20200613120312835" style="zoom: 50%;" />

​		使用代理服务器的理由有：利用缓存技术减少网络带宽的流量，组织内部针对特定网站的访问控制，以获取访问日志为主要目的等。

​		代理有多种使用方法，两种基准分类。一种是是否使用缓存，另一种是是否会修改报文。

**缓存代理**

代理转发响应时，缓存代理`Caching Proxy`会预先将资源的副本（缓存）保存在代理服务器上。

当代理再次接收到对相同资源的请求时，就可以不从源服务器那里获取资源，而是将之前缓存的资源作为响应返回。

**透明代理**

转发请求或响应时，不对报文做任何加工的代理类型被称为透明代理`Transparent Proxy`。反之，对报文内容进行加工的代理被称为非透明代理。

#### 网关

![image-20200614151802433](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200614151802433.png)

网关的工作机制和代理十分相似。而网关能使通信线路上的服务器提供非HTTP协议服务。

利用网关能提高通信的安全性，因为可以在客户端与网关之前的通信线路上加密以确保连接的安全。比如网关可以连接数据库，使用SQL语句查询数据。另外Web购物网站上进行信用卡结算时，网关可以和信用卡结算系统联动。

#### 隧道

隧道可以按要求建立一条与其他服务器的通信线路，届时使用SSL等加密手段进行通信。隧道的目的是确保客户端能与服务器进行安全的通信。

隧道本身不会去解析HTTP请求，请求保持原样转给之后的服务器。隧道会在通信双方断开连接时结束。

### 缓存

缓存指代理服务器或客户端本地磁盘内保存的资源副本。利用缓存可以减少对源服务器的访问，因此就节省了通信流量和通信时间。

**缓存有效期**

若判断缓存过期后，会向源服务器确认资源的有效性。若判断浏览器缓存失效，浏览器会再次请求新资源。

## HTTP的缺点

- 通信使用明文（不加密），内容可能会被窃听
- 不验证通信方的身份，因此有可能遭遇伪装
- 无法验证报文的完整性，报文有可能已被篡改

## HTTPS

**HTTPS = HTTP + 加密 + 认证 + 完整性保护**

加密：TLS / SSL

认证：CA证书

完整性保护：消息摘要

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613121135797.png" alt="image-20200613121135797" style="zoom:67%;" />



HTTPS无非是在传输层和应用层中间加了一层TLS，正是TLS紧跟当代密码学的步伐，尽全力的保障用户的安全。

![image-20200613121637962](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613121637962.png)HTTPS并非是应用层的新协议。只是HTTP通信接口部分用SSL和TLS协议代替而已。

HTTP直接和TCP通信，而使用SSL时，则演变成了先和SSL通信，再由SSL和TCP通信。

在采用SSL后，HTTP就拥有了HTTPS的加密、证书和完整性保护这些功能。SSL是当今世界上应用最为广泛的网络安全技术。

### 安全四要素

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613143349173.png" alt="image-20200613143349173" style="zoom:50%;" />

### TLS

可以看出在交互的过程中多了不少新东西，了解TLS,TLS由SSL握手协议，SSL修改密码规范协议，SSL警报协议，SSL记录协议组成。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613121649192.png" alt="image-20200613121649192" style="zoom:50%;" />

**SSL握手协议：**相对于三次握手

**记录协议：**记录为TLS发送接收数据的基本单位。它的自协议需要通过记录协议发出。如果多个纪录数据则可以一个TCP包一次性发出。

**警报协议：**类似HTTP状态码，通过反馈不同的消息进行不同的策略。

**变更密码规范协议：**告诉对方，从此刻开始，后续的数据将使用加密算法进行加密再传输。

### 对称加密与非对称加密

#### 对称加密

> 对称加密，顾名思义，加密方与解密方使用同一钥匙(秘钥)。具体一些就是，发送方通过使用相应的加密算法和秘钥，对将要发送的信息进行加密；对于接收方而言，使用解密算法和相同的秘钥解锁信息，从而有能力阅读信息。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613142720491.png" alt="image-20200613142720491" style="zoom: 50%;" />

#### 非对称加密

> 在对称加密中，发送方与接收方使用相同的秘钥。那么在非对称加密中则是发送方与接收方使用的不同的秘钥。其主要解决的问题是防止在秘钥协商的过程中发生泄漏。非对称密钥其实主要解决了密钥分发的难题。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613142801174.png" alt="image-20200613142801174" style="zoom:50%;" />

#### 混合加密

> 非对称加密算法，大多数是从数学问题演变而来，运算速度较慢。混合加密所谓取长补短。通信过程中使用RSA等解决密钥交换问题，然后使用随机数产生的在对称算法中的会话密钥，最后使用加密。对方使用私钥解密得到的密文取出会话秘钥，这样就实现了密钥交换。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613143228356.png" alt="image-20200613143228356" style="zoom:50%;" />

通过混淆加密等方式完成了机密性任务，作为Hack只需要伪造发布公钥或者作为之间人窃听密文。但是我们知道安全是四要素，还需要保证数据的完整性，身份认证等。

#### 摘要

> 摘要算法可以理解为一种特殊的"单向"加密算法，无密钥，不可逆。在平时项目中，应该大家都是用过MD5，SHA-1。但是在TLS中使用SHA-2。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613143506311.png" alt="image-20200613143506311" style="zoom:50%;" />![image-20200613143520916](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613143520916.png)<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613143506311.png" alt="image-20200613143506311" style="zoom:50%;" />![image-20200613143520916](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613143520916.png)

### CA证书

数字证书认证机构处于客户端和服务器双方都可信赖的第三方机构的立场上。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200614165534736.png" alt="image-20200614165534736" style="zoom:80%;" />

HTTPS的整体过程分为证书验证和数据传输阶段。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613154430937.png" alt="image-20200613154430937" style="zoom:50%;" />

#### CA证书内容

- 颁发机构信息

- 公钥

- 公司信息

- 域名

- 有效期

- 指纹

- ……

#### 为什么需要 CA 证书

​		HTTP 协议被认为不安全是因为传输过程容易被监听者勾线监听、伪造服务器，而 HTTPS 协议主要解决的便是网络传输的安全性问题。

​		首先我们假设不存在认证机构，任何人都可以制作证书，这带来的安全风险便是经典的 “中间人攻击” 问题。 

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613155526268.png" alt="image-20200613155526268" style="zoom: 67%;" />

#### 如何验证证书合法性

浏览器发起 HTTPS 请求时，服务器会返回网站的 SSL 证书，浏览器需要对证书做以下验证：

- 验证域名、有效期等信息是否正确。证书上都有包含这些信息，比较容易完成验证；

- 判断证书来源是否合法。每份签发证书都可以根据验证链查找到对应的根证书，操作系统、浏览器会在本地存储权威机构的根证书，利用本地根证书可以对对应机构签发证书完成来源验证； 
- 判断证书是否被篡改。需要与 CA 服务器进行校验；

- 判断证书是否已吊销。通过CRL（Certificate Revocation List 证书注销列表）和 OCSP（Online Certificate Status Protocol 在线证书状态协议）实现，其中 OCSP 可用于第3步中以减少与 CA 服务器的交互，提高验证效率

### HTTPS请求建立连接过程

![image-20200614182548052](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200614182548052.png)

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613143545745.png" alt="image-20200613143545745" style="zoom:50%;" />

注意：

- 首先通过非对称加密建立通信过程
- 在握手阶段，为什么使用3个随机数，一方面防止「随机数 C」被猜出，另一方增加Session key随机性
- Client发出支持的「对称/非对称加密」算法
- server返回选用的「对称/非对称加密」算法
- Client对算法进行确认
- Server对算法进行确认

![image-20200614183506815](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200614183506815.png)

根据wireshak结果，对TLS进一步剖析。TCP三次握手建立连接，作为礼貌，Client先打招呼"Client Hello"。里面包含了Client的版本号、所支持的密码套件和随机数，如下图所示

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613144134261.png" alt="image-20200613144134261" style="zoom: 80%;" />

Server端表示尊重，回复"Server Hello",同时进行版本校对，给出随机数(Server Random)，从Client算法列表中选择一个密码套件，在这里选择的"TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256"。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613144157020.png" alt="image-20200613144157020" style="zoom:80%;" />

这里的"TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256"什么意思呢

> 密码套件选择椭圆曲线加RSA、AES、SHA256

双方通过证书验证身份。因为本机服务器选用了ECDHE算法，为了实现密钥交换算法，它会发送证书后把椭圆曲线的公钥（Server Params）连带"Server Key Exchange"消息发送出去。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613145332081.png" alt="image-20200613145332081" style="zoom: 80%;" />

意思是，刚才混合加密套件比较复杂，给你个算法参数，好好记住，别弄丢了。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613145410346.png" alt="image-20200613145410346"  />

随后服务端回复"hello done"告知打招呼完毕

打完招呼完毕后，客户端对证书进行核实。然后根据密码套件也生成椭圆曲线的公钥，用"Client Key Exchange"消息发给服务器

![image-20200613145456577](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613145456577.png)

此时客户端和服务端都有了密钥交换的两个参数(Client Params、ServerParams），然后通过 ECDHE 算法算出了一个新的值，叫“Pre-Master”

有了主密钥和会话密钥，客户端发送“Change Cipher Spec”和“Finished”消息，最后将所有消息加上摘要发送给服务器验证。

服务器同样发送“Change Cipher Spec”和“Finished”消息，握手结束，开始进行HTTP请求与响应

## 域名

> 我们知道域名的出现让我们更容易记忆，按照"."分割，越靠近右边级别越高。域名本质是一个名字空间系统，采用多级域名的方式区分不同的国家，公司等，作为一种身份的标识。

**根域名服务器（Root DNS Server）**：管理顶级域名服务器，返回“com”“net”“cn”等顶级域名服务器的 IP 地址；

**顶级域名服务器（Top-level DNS Server）**：管理各自域名下的权威域名服务器，比如com 顶级域名服务器可以返回 apple.com 域名服务器的 IP 地址；

**权威域名服务器（Authoritative DNS Server）**：管理自己域名下主机的 IP 地址，比如apple.com 权威域名服务器可以返回 www.apple.com 的 IP 地址**

## HTTP识别（还原）

> 这里说的识别，通过代码层面(libpcap封装)实现HTTP的识别，也能进一步体现TCP/IP协议栈的分层特性。先看回忆一下IP头部格式。

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200613151145272.png" alt="image-20200613151145272" style="zoom:67%;" />

​		注意头部中的协议字段，如果此字段值为0x0600则为TCP分组。当知道了是TCP分组后，是不是可以通过TCP头部中端口(80)就可以判断为HTTP呢，不能的，很多情况都会使用动态端口的方式进行部署。此时可以通过HTTP中的关键字进行判断。如果为HTTP，再通过头部字段中的"Content-type"，charset等确认文本信息，编码方式，最后采用解码算法进行还原。

## HTTP面试题测试

- Get和Post区别

- HTTP与HTTPS区别

- HTTP通信过程

- 游览器输入一个地址。到页面展示中间经历了哪些步骤？

- cookies机制和session机制的区别：

- HTTP请求报文与响应报文格式

- 一次完整的HTTP请求所经历的7个步骤

- HTTP优化方案

- 不同版本的HTTP区别

- HTTP优点缺点

- URI和URL的区别

- 如何判断是否为http

- HTTP 1.1引入分块传输编码提供了以下几点好处

- 长连接与短连接的区别，以及应用场景

- 常见web攻击

- 站内跳转和外部重定向有何区别

- HTTP的keep-alive是干什么的？

- 关于Http 2.0 你知道多少？

- 讲讲304缓存的原理

- HTTP与RPC异同



## Web的攻击技术

### XSS

跨站脚本攻击（Cross-Site Scripting XSS）是指通过存在安全漏洞的Web网站注册用户的浏览器内进行非法的HTML标签或JavaScript进行的一种攻击。

跨站脚本攻击可能造成以下影响：

- 利用虚假输入表单片区用户个人信息。
- 利用脚本窃取用户的Cookie值，被害者在不知情的情况下，帮助攻击者发送恶意请求。
- 显示伪造的文章或图片

### SQL注入攻击

SQL注入（SQL Injection）是指对Web应用使用的数据库，通过运行非法的SQL而产生的攻击。该安全隐患有可能引发极大的威胁，有时会直接导致个人信息及机密信息的泄漏。

Web应用通常会用到数据库，当需要对数据库表内的数据进行检索、添加、删除等操作时，会使用SQL语句连接数据库进行特定的操作。如果在调用SQL语句的方式上存在疏漏，就有可能执行被恶意注入非法SQL语句。

SQL注入攻击可能会造成以下影响：

- 非法查看或篡改数据库内的数据
- 规避认证
- 执行和数据库服务器业务关联的程序

### DoS攻击

DoS攻击（Denial of Service attack）是一种让运行中的服务呈停止状态的攻击。有时也叫做服务停止攻击或拒绝服务攻击。DoS攻击的对象不仅限于Web网站，还包括网络设备及服务器等。

主要有以下两种DoS攻击方式。

- 集中利用访问请求造成资源过载，资源用尽的同时，实际上服务也就呈停止状态
- 通过攻击安全漏洞使服务停止

## websocket

