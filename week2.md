4.19

[MRctf2021](https://inanb.github.io/2021/04/18/MRctf2021/)

[FBCTF2019-RCEService](https://inanb.github.io/2021/04/18/FBCTF2019-RCEService/)

[WUSTCTF2020-颜值成绩查询](https://inanb.github.io/2021/04/16/WUSTCTF2020-颜值成绩查询/)

[EasyBypass](https://inanb.github.io/2021/04/15/EasyBypass/)

[EasyBypass](https://inanb.github.io/2021/04/15/EasyBypass/)

这周事确实挺多……依旧写了五道题，学习学习新知识，准备看看刚出的mrctf的wp……

有点找不到方向……下面是web提纲的完成

##### hacker101.0

https://www.bilibili.com/video/av75418689/

1.本视频一开始介绍了哪两个工具，他们的作用分别是什么？为什么作者会推荐firefox，它的优点是什么？（5分）

> Bure Proxy & Firefox
>
> Bure Proxy可以对HTTP和HTTPS的流量包进行监控和修改，是业内流行工具。
>
> Firefox在做请求处理时，可以通过它轻松实现浏览器的代理设置，而无需对系统做其他的设置。设置后，它捕获的即是我们所关心的web应用流量而非系统的其他流量。Firefox还包括了一些开发调试的工具，可以方便的查看修改cookie信息，检查DOM……这就是为什么作者推荐Firefox。当然，也可以用Chrome和Burp一起配合工作。

2.本视频中体现了哪些攻防上的哲学观点？作者希望你养成什么样的思维？这些思维在帮助你挖掘漏洞的时候有什么帮助？结合你的经历与视频内容谈谈你的看法。（10分）

> 我们应当站在attacker的角度来思考问题，而不是defender。应该了解攻击者对攻击目标的攻击套路和攻击思维。对一些测试的应用程序来说，了解他的最好方法是大胆点击它的每个功能按钮来加深对他的功能认识并发现它的功能弱点。
>
> 作者还强调，攻防双方在安全职责任务上存在一种不平衡。通常对于防护者来说，必须去发现系统中所有存在的漏洞；对于攻击者，只需发现少量或一个漏洞即可。攻击者比防护者明显更具优势。实战可能方法很简单，但是不可能发现的了所有漏洞。精力是有限的，我们必须有轻重缓急之分，尤其要学会识别一些高风险漏洞问题，最大化降低攻击影响或一些可能的情况。换位思考评估攻击者最想获得什么东西，这是重要的安全技能培养方法。
>
> 对每个功能点进行熟悉并为之可能产生的漏洞分类与评级，考虑不同功能点的综合利用，这有助于理清逻辑并尽可能先发现更加危险的漏洞。

3.审计以下代码：

```php
<?php
if(isset($_GET[ ' name ' ])){
echo "<h1>Hello {$_GET['name']} !</h1>";
}
?>
<form method="GET">
Enter your name: <input type="input" name="name"><br>
<input type=" submit">
```

本段代码涉及到客户端，服务端以及通信协议。运行在客户端的代码主要有HTML以及javascript，由浏览器核心负责解释 通信协议为HTTP协议，有多种格式的请求包，常见的为POST与GET 运行在服务端的代码为php，由php核心负责解释。 用户端与服务端通过HTTP通信协议进行交互。 

- 那么，以上代码中，哪些部分属于客户端的内容，哪些属于服务端的内容？（1分） 

- 客户端是通过传递什么参数来控制服务端代码的？（1分） 

- 客户端通过控制该参数会对服务端造成什么影响，继而使得客户端本身收到影响，从而造成了什么漏洞？如果是xss漏洞，具体又是什么类型的xss漏洞，为什么？（3分）

  

> ​		https://blog.csdn.net/wang404838334/article/details/78449149                                     		
>
> 服务器web：我们把提供（响应）服务的计算机称作服务器（Server），也叫服务器端。       		
>
> 客户端web：接受（请求）服务的计算机称作客户机（Client），也叫客户端。                                             		
>
> ①当用户在浏览器地址中输入要访问的PHP页面文件名，然后回车就会触发这个PHP请求，并将请求传送化支持PHP的WEB服务器。                                                                                        ②WEB服务器接受这个请求，并根据其后缀进行判断如果是一个PHP请求，WEB服务器从硬盘或内存中取出用户要访问的PHP应用程序，并将其发送给PHP引擎程序。                                      		                                                                                                                                     ③PHP引擎程序将会对WEB服务器传送过来的文件从头到尾进行扫描并根据命令从后台读取，处理数据，并动态地生成相应的HTML页面。                                                                                        		                                                                                                                                    ④PHP引擎将生成HTML页面返回给WEB服务器（apache）。WEB服务器再将HTML页面返回给客户端浏览器，最后一个完整的页面基于通过浏览器展现在我们眼前。                		这里内嵌的php语句应当是服务端的内容，而html的渲染是属于客户端浏览器上渲染的。                                                                                                                                                                                                                                         客户端在html显示的表单通过get提交了一个name参数，服务端识别这个以get方法提交的参数，在`echo "<h1>Hello {$_GET['name']} !</h1>";`中hello后的值被控制。
>
> ​		这段参数没有进行任何的过滤，使得hello后的html字段可以由客户端任意控制，形成xss漏洞。由于内容只与用户提交的参数有关，需要欺骗用户自己去点击链接才能触发XSS代码（服务器中没有这样的页面和内容），故为反射型xss漏洞。

4.思考：现实中如何利用xss漏洞实施攻击，我们应该如何预防？（1分）

> 现实中实施攻击……关键在于参数的隐藏，可以进行url编码；如果还是可疑，可以利用网页上的网址缩短工具或绑定到一个看起来正常的网址……大概？攻击的投放可以通过邮件、广告、借助评论或图片上传链接、钓鱼网站……（都没实战过
>
> 预防……可以通过严格的过滤和应用CSP来阻止xss的产生，HttpOnly好像可以防止cookie窃取……