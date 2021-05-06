---
typora-root-url: ..\HexoBlog\source
---

5.6

[HarekazeCTF-2019-encode_and_encode](https://inanb.github.io/2021/05/04/HarekazeCTF-2019-encode-and-encode/)

[NCTF2019-SQLi](https://inanb.github.io/2021/05/04/NCTF2019-SQLi/)

[SWPUCTF-2018-SimplePHP](https://inanb.github.io/2021/05/04/SWPUCTF-2018-SimplePHP/)

[GYCTF2020-Ezsqli](https://inanb.github.io/2021/05/04/GYCTF2020-Ezsqli/)

buu刷题，做iscc，蓝帽签到……马上去复现……

学了点css，把之前做的取反页面美化一下挂到服务器上，正在学JavaScript，试着写一些不错的效果练手

了解了jsjiami.v6、selenium、sql注入的一些操作、phar反序列化……

下面是web提纲三：

##### hacker101.2

中文视频：

[【FreeBuf字幕组】Hacker101白帽黑客进阶之路-Web工作机制](https://www.bilibili.com/video/BV1MJ411D75M?from=search&seid=11355973279630099592)

1.http报文的结构是什么？（1分） 

> 一个HTTP请求报文由请求行（request line）、请求头部（request header）、空行和请求数据4个部分构成。
>
> **请求行**数据格式由三个部分组成：请求方法、URI、HTTP协议版本，他们之间用空格分隔。                                                     
>
> ```
> GET / HTTP/1.1 					#请求方法为GET，请求路径为根目录，1.1版本
> ```
>
> **请求头部**紧跟着请求行，该部分主要是用于描述请求正文                                                                           
>
> ```
> Host: hackerone.com                                                                Cache-Control: max-age=0
> Upgrade-Insecure-Requests: 1
> User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.121 Safari/537.36
> Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
> Accept-Encoding: gzip, deflate
> Accept-Language: zh-CN,zh;q=0.9
> Connection: close		##主要是用于说明请求源、连接类型、Cookie信息、可处理内容等
> ```
>
> **请求正文**和请求头部通过一个空行进行隔开，一般用于存放POST请求类型的请求正文
>
> ```
> username=admin
> ```

2.什么是crlf？在http报文的哪个位置。（1分） 

> | **缩写** | **ASCⅡ转义** | **系统**               | **ASCⅡ值** |
> | -------- | ------------ | ---------------------- | ---------- |
> | CR       | \r           | MacIntosh（早期的Mac） | 13         |
> | LF       | \n           | Unix/Linux/Mac OS X    | 10         |
> | CR LF    | \r\n         | Windows                |            |
>
> CR：Carriage Return，对应ASCII中转义字符\r，表示回车
>
> LF：Linefeed，对应ASCII中转义字符\n，表示换行
>
> CRLF：Carriage Return & Linefeed，\r\n，表示回车并换行
>
> Windows操作系统采用两个字符来进行换行，即CRLF；
>
> Unix/Linux/Mac OS X操作系统采用单个字符LF来进行换行；
>
> MacIntosh操作系统（即早期的Mac操作系统）采用单个字符CR来进行换行。
>
> ---------------
>
> **Burp**中，点击\n即可看到报文中crlf的详细位置，据我目前的经验，crlf可用于csrf和http走私
>
> ……

3.解释下这几个头的含义（5分）： 

<img src="/images/week4/image-20210506102725317.png" alt="image-20210506102725317" style="zoom:80%;" />

> head头：
>
> Host：表明请求将要发送至的实际主机
>
> Accept：客户端浏览器可处理的MIME类型，设置用来指定响应类型
>
> Cookie：包含客户端向服务器传递的cookie数据
>
> Referer：表明请求具体来源于哪个页面的链接
>
> Authorization：通常用来做一些token认证

4.cookie具有哪些特点，不同的域名和子域名对cookie有怎样的权限？Cookie的Secure和 HTTPOnly这两个flag分别有什么作用？请结合xss攻击来进行说明（3分） 

> 若Set-Cookie指定Domain为xxx.com，那么xxx.com的子域名同样能够访问获取到这个Cookie。若指定Domain为某子域名(sub.xxx.com)，那么只有该子域名及其下级子域名才可以访问获得Cookie。
>
> Cookie的Secure属性：这代表Cookie只能在HTTPS协议中传输
>
> Cookie的HTTPOnly属性：确保cookie只能通过Web请求传输，而不能用JavaScript来读取访问。原本可用的document.cookie方法的JS脚本在打开HTTPOnly后无法读取cookie，cookie只在请求中传输。

5.简述本视频提到的xss绕过web防火墙的方案（5分）

> 利用浏览器与WAF对html解析方式不同的特点。\<script/xss src=xxxxxx\>在浏览器中，/会被解析为%20，而WAF仅仅认为这是一个script/xss样式。这样就绕过了WAF，而浏览器中该标签得以执行。

6.内容嗅探是什么？主要有哪些类型？请分别举例，主要用途是什么？在什么情况下可以利用这些漏洞？为什么facebook等网站需要使用不同的域名来存储图片？（5分）

> **内容嗅探（Content Sniffing）**
>
> 浏览器在显示响应回来的请求文件或网页时，若不知道该文件或网页的具体文件内容（Content-Type），会启动内容嗅探机制。如：**MIME Sniffing**，它的原理是浏览器回显自动探测未知格式的请求文件类型。另：**Encoding Sniffing**，浏览器会自动检测未知格式文件的编码类型。通过对内容编码格式的一一匹配进行嗅探，后在浏览器中进行相应的解析显示。
>
> 在请求响应不具备恰当的MIME类型时，浏览器的具体检测和解析机制可能会导致对文件或图片的不正确解析，导致此类漏洞的发生与利用。
>
> Encoding Sniffing则利用浏览器的自动识别而WAF难以支持的特点(如：UTF-7、UTF-32)完成攻击，之前做过一个xml转码完成攻击的题。
>
> facebook使用子域来托管图片，防止因此类漏洞造成的xss攻击。即使攻击完成，获取到的也可能仅仅是子域下的一个cookie。

7.同源策略是什么？限制是什么？浏览器在遇到哪两种情况的时候会用到同源策略？如何放松SOP限制？放松SOP限制会对浏览器插件安全造成怎样的破坏？

> **同源策略（SOP）**
>
> https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy
>
> Web安全的基石。同源策略目的在于防止Web交互间的不安全行为。同源策略限制了跨域的资源访问。
>
> **限制**：协议、端口号、主机（域名）必须相同。
>
> 不同域之间是通过XML的HTTP请求来实现交互的，也就是AJAX请求。SOP会限制来自iframe和windows属性的跨域访问和AJAX请求？所以当我们打开一个带有JS脚本的新窗口，由于SOP的影响，那么就无法对该窗口中的DOM元素实现访问。
>
> **弱化SOP**：可以通过设置不同域之间的document.domain脚本来实现。这种方式可以让本质上不同源的子域实现互相通信与交互。或者，通过postMessage方法实现跨窗口通信。
>
> **破坏**：跨域跨窗口的信息验证很少存在，而Web插件的内部消息处理经常是错误的。如：Chrome的拓展插件就存在SOP绕过漏洞。Chrome的拓展插件在浏览器准备的sandbox中受到了很多限制，所以这些插件常使用postMessage进行设计。当我们向插件发送信息时，可以通过跨窗口通信绕过SOP，实际上这已经破环了浏览器准备的sandbox，是由SOP弱化导致的漏洞。

8.csrf是什么？如何设计规避csrf？视频中提到的错误的csrf配置方法是什么？ 

> **CSRF（跨站请求伪造）**
>
> 攻击者挟持受害者去访问由攻击者控制的网站。并以受害者身份执行请求、提交等恶意操作。
>
> ```html
> <body onload="document.forms[0].submit()">
> <form action="xxxx" method="POST">
> 	<input type="hidden" name="amount" value="1000000">
> 	<input type="hidden" name="to" value="1625">
> </form>
> </body>
> ```
>
> 这样的请求直接发送至后端，而后端无法分请请求是否是伪造的。
>
> 我们可以使用CSRF-Token来规避此类攻击。Token随机生成并嵌入表单中（CSP……?哦，不一样……
>
> 如：
>
> ```html
> <form action="xxxx" method="POST">
> 	<input type="hidden" name="csrf" value="xxxxxxxxxxxxxxxxxxxxxxxxxxx">
> 	<input type="submit">
> </form>
> ```
>
> 视频中提到有Web应用通过加载csrf.js来生成token，泄露了token的生成方式，这是一种自毁的防御措施。

附加题：5、6两点主要利用的是由于服务端和客户端对同一信息的处理方式不同造成的漏洞，你还能举出相似的例子么？（1分）

> …………

