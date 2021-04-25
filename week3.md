4.25

[RCTF2015-EasySQL](https://inanb.github.io/2021/04/25/RCTF2015-EasySQL/)

[网鼎杯-2018-Comment](https://inanb.github.io/2021/04/24/网鼎杯-2018-Comment/)

[CISCN2019-华北赛区-Day1-Web5-CyberPunk](https://inanb.github.io/2021/04/23/CISCN2019-华北赛区-Day1-Web5-CyberPunk/)

[CISCN2019-总决赛-Day2-Web1-Easyweb](https://inanb.github.io/2021/04/23/CISCN2019-总决赛-Day2-Web1-Easyweb/)

写了几道题，记录了这四道……

学习了挺多东西，主要是sql的二次注入的应用场景和一些trike

下面是提纲的完成：

##### hacker101.1

2.观看视频二，回答下列问题：

视频二：https://www.hacker101.com/sessions/pentest_owasp 

1.目前owasp的十大web安全漏洞是哪些？这些漏洞排名是按照漏洞的严重程度排序的还是按照漏洞的常见程度排序的？（2分） 

> [中文PDF-2017](https://wiki.owasp.org/images/d/dc/OWASP_Top_10_2017_中文版v1.3.pdf)
>
> 1. **注入漏洞：**[Injection](https://www.immuniweb.com/resources/owasp-top-ten/#Injection)
>
> 2. **身份认证失效：**[Broken Authentication](https://www.immuniweb.com/resources/owasp-top-ten/#Authentication)
>
> 3. **敏感数据泄露：**[Sensitive Data Exposure](https://www.immuniweb.com/resources/owasp-top-ten/#Data)
>
> 4. **XML外部实体漏洞：**[XML External Entities (XXE)](https://www.immuniweb.com/resources/owasp-top-ten/#XML)
>
> 5. **访问控制失效：**[Broken Access Control](https://www.immuniweb.com/resources/owasp-top-ten/#Access)
>
> 6. **安全配置错误：**[Security Misconfigurations](https://www.immuniweb.com/resources/owasp-top-ten/#Misconfigurations)
>
> 7. **跨站脚本攻击：**[Cross-Site Scripting (XSS)](https://www.immuniweb.com/resources/owasp-top-ten/#XSS)
>
> 8. **不安全的反序列化：**[Insecure Deserialization](https://www.immuniweb.com/resources/owasp-top-ten/#Deserialization)
>
> 9. **使用含有已知漏洞的组件：**[Using Components with Known Vulnerabilities](https://www.immuniweb.com/resources/owasp-top-ten/#Vulnerabilities)
>
> 10. **日志和监控不足：**[Insufficient Logging and Monitoring](https://www.immuniweb.com/resources/owasp-top-ten/#Insufficient)
>
>     ***OWASP Top 10 is a ranking of the ten most dangerous information security risks for web applications, compiled by a community of industry experts. For each point of the rating, the risk is calculated by experts based on the[OWASP Risk Rating Methodology](https://owasp.org/www-project-risk-assessment-framework/)and includes an assessment of Weakness Prevalence, Weakness Detectability and Exploitability, as well as the criticality of the consequences of their operation or Technical Impacts.***
>
>     **前10大风险项是根据流行数据选择和优先排序，并结合了对可利用性、可检测性和影响程度的一致性评估而形成。**

2.请翻译一下credential stuffing（1分） 

> ​    ***one that's really common from a pentesting perspective is what we call credential stuffing. And say that we go out we grab data from breached data that's out there so an account that's been compromised the list shows up on the internet and now, the hackers have access to usernames and passwords and say we are testing in some website.***
>
> ​    ***we just send usernames and passwords to an application ,and there's no threat detection we're just able to pound a login form or even use something like password spraying where we just take something like spring 2019 or summer 2020 exclamation and just throw common credentials out there and see if it sticks if we're able to continuously brute force  an application without anything preventing us or any account lockouts.***
>
> emm，实际上就是撞库吗……
>
> 利用已泄露的账户信息在多个平台进行尝试，这种尝试很难被检测出来。不同的用户名与密码，甚至于手机号，身份证号等等敏感信息进行组合来尝试登录。可能用户在多个平台的账户与密码是相同的，这样就十分危险。
>
> [**Credential Stuffing Attack**](https://blog.csdn.net/weixin_44549063/article/details/113518965)
>
> [Beyond credential stuffing](https://blog.csdn.net/still_night/article/details/104837926)

3.为什么说不充分的日志记录(insufficient logging)也算owasp十大漏洞的一种？他的危害性如何（2分） 

>  之所以有很多入侵行为发生，其中原因之一是没有行为监管，或监管不充分。入侵者r进入系统然而没有日志记录这一行为，或日志记录不充分。
>
> 所以，在做一些项目时，检查日志与监控是十分重要的。不充分的日志记录与监管可能会导致很多入侵行为的发生。

4.请翻阅一下owasp testing guide，以及owasp testing guide check-list，视频说怎么结合这两个文档来学习渗透测试？ 结合你平时渗透过程中的经验，谈谈你的感想。（3分） 

> [中文版testing-guide](https://kennel209.gitbooks.io/owasp-testing-guide-v4/content/zh/frontispiece/index.html)
>
> https://github.com/tanprathan/OWASP-Testing-Checklist
>
> 从testing guide中，我们可以了解到所有的不同的渗透测试一个网站应用的方法。check-list可以提供我们在测试中所有要检查的点，并为这些点提供测试工具。check-list中的测试点可拿到testing guide中检索得到更为详尽的信息与总结，search engine、operator……and step by step。
>
> emmm，速查与教科书的关系……？渗透测试是开卷的，速查与教科书就显得十分重要……                                                                                                                               我们需要清楚应用工作的方式，熟悉测试的逻辑，而check-list和testing-guide负责帮助我们理顺逻辑并记忆基础测试点。
>
> 从思考与逻辑出发，最终落实到check-list and testing-guide。check-list and testing-guide帮助我们查漏补缺。

5. you are only as good as you notes   you are only as good as things you can refer to结合这两句话谈谈你的感想。（2分）

> 挺好……owasp提供了很好的资源，各种test-guiding，hacker101也提供了很好的学习资源（包括英语……
>
> 写博客这么长时间，感觉也确实有用。包括资料查找、记录payload、各种知识点……我们所记录的、可参考的东西是记忆可靠的延伸。