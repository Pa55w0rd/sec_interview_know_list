**被虐经历**

**知识清单**

- **xss防御相关**
    - 编码防御
        - html实体编码
        - js编码
        - url编码
        - **输出点在标签内部应该怎么防御？**
        - **输出点在标签外部应该怎么防御？**
        - **三种编码的关系，以及什么地方用到什么编码**
        - **浏览器解码的过程**
        - **开启httponly的情况下如何利用XSS漏洞**
        - xss输出在注释里怎么利用 （换行符利用）
        - 如果页面是gbXXX 如何利用宽字节进行xss利用
        - 在xss利用过程中 讲 < 写成 \u003c 可以绕过安全防护 请问这个安全防护的思路是什么
        - XSS防御的7大原则
        - 心伤的瘦子XSS教程
        - **富文本防御XSS的思路**
        - 如果 ”javascript“字符串被过滤了，你的绕过思路是什么？
            - 大小写
            - Tab 空格 回车(%0a)
            - 插入 “/**/” \0 \ 
            - 编码
        - CSS中expression表达式可以插入xss吗
        - 如果输出点在css style标签中 要注意expression表达式 import等之类

        - **XSS编码方案(方案普遍性很高，具体要看业务场景)**(大家自行思考为什么这么做？)
            - 当输出点出现在HTML标签属性：
            
             ```
                < -> &lt;
                > -> &gt;
                & -> &amp;
                " -> &quot;
                ' -> &#39
             ```
            
            
            - 当输出点出现在<script>标签中。这种情况相当危险，不需要考虑xss触发，只需要考虑编写js即可

            
            ``` 
                ' -> \';
                " -> \";
                \ -> \\;
                / -> \/;
                (换行符) -> \n;
                (回车符) -> \r;
            ```                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
            
            - 当输出点出现在body中
            
            ```
                < -> &lt;
                > -> &gt;
                & -> &amp;
                " -> &quot;
                ' -> &#39
            ```
            
            - 当输出点出现在js事件中(onClick="你的代码")
            
            ```
                < -> &lt;
                > -> &gt;
                & -> &amp;
                " -> &quot;
                ' -> &#39
                \ -> \\;
                / -> \/;
                (换行符) -> \n;
                (回车符) -> \r;
             ```
             
             - 输出在URL属性中<script src="你的代码">
                - URL编码
             
        - **推荐阅读**
            - [防御XSS攻击的七条原则](http://www.freebuf.com/articles/web/9977.html) 
            - [深入理解浏览器解析机制和XSS向量编码](https://www.cnblogs.com/b1gstar/p/5996549.html)   

- **CSRF相关**
    - 只校验Refer可以吗
    - token放在哪里？放在cookie里可以吗？不失效可以吗？
    
- **XXE漏洞相关**
    - XML文件格式
    - XXE漏洞利用的方式
    - XXE漏洞修复方案
    - XXE漏洞
    
- **sql注入漏洞相关**
    - 注入的类型
        - 普通注入(有数据库回显)
            - 数字型注入
            - 字符型注入
            
        - 盲注
            - 什么是盲注
            - 三种类型
                - 基于布尔类型的盲注
                    - left()
                    - substr()
                    - version()
                    - ascii()
                    - user()
                    - database()
                    - @@basedir
                    
                - 基于报错的盲注
                    - double数值类型超出范围
                    - bigint溢出
                    - xpath函数报错注入
                    - extractvalue()
                    - floor() rand() group by
                    
                - 基于时间延时的盲注
                    - sleep()
                    - benchmark()
        
        - 堆叠注入
        
        - order by注入
                    
        
        - 宽字节注入
            - 1.php?id='1%df反斜杠' (其中反斜杠为%5c,%df%5c在GBK编码下可以变成'蓮' 类似于这个字，那个字我不会打，原谅我没文化) 变成 1.php?id='1蓮'
            - 将 \' 中的 \ 过滤掉，例如可以构造 %**%5c%5c%27 ，后面的 %5c 会被前面的 %5c 注释掉。
            - 宽字节注入的修复方案
            
        - URLDecode二次注入
            - 浏览器编码完之后WebServer会自动解码的，如果后端程序误用urldecode函数会造成此类情况(1.php?id=1%2527==>(WebServer)1.php?id=1%27==>(urldecode)1.php?id=1')
        
    - 检查注入的思路
        - 通过加单引号 双引号看看是否有报错。
            - 有报错（不一定有注入）：
                - 通过拼接语句来进行状态判断
                    - and ,or
                    
            - 没有报错（有可能是盲注）：
                - 如果关闭错误回显的话 基于报错注入就不可能了。
                - 构造语句利用延时注入和联合注入进行攻击
                    - sleep benchmark extractvalue
                    
        - 看状态码(正常的话是200 注入的话可能会存在500 302等)
        
        - 特殊注入需要额外观察：
            - 宽字节注入
            - url二次注入
            
    - mysql注释
        - '--'
        - '#'
        - /* */ 多行注释
    
    - 掌握
    - 方案(参数化查询会有问题吗？)
    - ORM
    - 如果检测被拦截了怎么绕过（比如sleep被waf拦了）
    - Mysql的提权都有哪些，UDF提权的原理。
    - Sqlmap原理
    - 在SSM框架中，Mybatis注入是什么情况造成的？#{},${}有什么区别，mybaties的预编译是如何实现的。
    - 什么情况下Mybatis必须使用${},为什么只能使用${}。
    

- **CRLF注入**


    
- **SSRF**
    - 说一个容易出现SSRF漏洞的场景
    - 如果过滤了以http开头的协议怎么绕过
    - 利用DNS重绑定来绕过SSRF的原理。
    - SSRF的地方也是可以跟伪协议在一起利用的。
    

- **Waf绕过**
    - 绕过的本质是什么，是在寻找2个或者多个集合之间特性的差异。利用这些差异点进行绕过。
    - 架构层绕过WAF
    - 资源限制角度绕过WAF
    - 协议层面绕过WAF的检测
    - 规则层面的绕过
        - SQL注入
            - 注释符绕过
            - 空白符绕过
            - 函数分隔符
            - 编码相关
        - 文件包含
            - 相对路径 
            - 绝对路径
    
- **DDOS防御相关**
    - DDOS攻击的类型
    - DDOS云防御的方案
    - DDOS反射攻击基于的协议？为什么基于这个协议？
        
    

    
- **android逆向相关**
    - 脱壳的原理
    - 如何查壳
    - smali语法
    - davilk指令
    - 如何防打包
    - 如何防签名校验
    - Android App加固原理分析(说一个加固的思路)
    - 防御思路
        - 对抗静态分析
            - 代码混淆技术 ProGuard
            - NDK保护
            - 壳
        - 对抗动态调试 
            - android:debuggable="false"，让程序不可调试
            - android.os.Debug.isDebuggerConnected()
            - 检测模拟器
        - 防止重编译
            - 检查签名 Eclipse自带的调试版密钥文件生成的apk文件的hash值,与上面的函数获取的hash比较
            - 检测Dex文件的Hash
    - android 反调试原理
        - 检测/proc/pid/status文件中的tracePID 如果不为0的情况，就是说明有程序正在进行反调试，该值为调试的进程的pid。一般在native层会fork一个子进程来循环的读取/proc/pid/status文件中的tracePID字段，如果不为0，直接exit
    - 绕过反调试的思路
        - 在JNI_ONLOAD下断点
        - 修改android内核。
    
    - android加壳
        - 说一说每一代壳的主要技术和思路。
            - 网络上对android壳的发展历史有着多个版本，有的是认为发展到现在经历了4代壳，有的则认为是5代壳。不过这些都不重要，相关的技术和思路都提现出来。这里我以5代为版本说一下我自己的理解。
                - 一代壳最大的特点是动态加载，思路比较简单，就是在静态的情况下不让你看见整个dex文件，然后主动运行壳的入口，然后自定义类加载器进行加载，运行。这种脱壳的核心思路就是在内存中找到一个比较合适的时机，对dex文件进行dump。比如dvmDexFileOpenPartial函数下断点，这个函数是优化dex文件的函数，第一个参数就是dex文件指针。
                
                - 二代壳主要是实现了不落地加载。这个不落地是指文件没有流入文件系统，直接在内存中动态生成，但是由于dex文件依然是成片存在于内存中的，所以核心思路还是在合适的时机dump内存即可。由于不同厂商分别不同实现了相关函数，或者对一代壳的dvmDexFileOpenPartial函数进行了相应的保护。一般二代壳在mmap(),memcpy()等下断点依旧可以dump出dex文件。
                
                - 三代壳主要是在前2代的基础上增加了代码抽取。这样在你面前呈现出来的代码很多函数都是空的。那是因为在此之前你的dex文件中很多关于method_id部分中代码用00来填充，真正的代码隐藏在别处，在这个函数被执行之前，主动还原被填充的00 ins部分。这种壳脱壳可以借鉴dexhunter工具。
                
                - VMP壳和Ollvm混淆 目前比较领先的加固方案。
     

        - dex文件的加载流程:
            - 见我的另外一篇分析文章。 从android源码看脱壳。 (https://tiaotiaolong.net/2019/07/05/从android源码看脱壳/)
            
    
- **浏览器安全**
    - https协议握手过程
    - burp 中间人攻击的原理
    - 分别说3个对称加密 非对称加密 哈希算法 
    - CSP
    - 除了公私钥密码加密体系还有其他可以确保传输安全的吗？
    - 简述一下同源策略
    - 同源策略下如何从 a.baidu.com 去获取 www.baidu.com 的 Cookie
    - 网页木马的工作原理
    - 同源策略下如何解决跨域请求 (分别说说原理和局限性)
        - document.domain 
        - jsonp
        - CORS
    - cookie遵守同源策略吗？(其实是不完全遵守的)。
    - jsonP安全
    - CORS的整个流程能说一下吗？
    - CORS跟CSRF之前有什么联系吗？能把这个问题想明白，前端跨域这块算是可以了。
    
- **PHP安全**
    - PHP的那些魔法函数造成的安全问题(当然了，你也可以说程序员不了解php的语言特性 哈哈哈哈)
        我个人觉得这些魔法函数是代码审计的基础，这也是为什么代码审计都喜欢挑php来捏，语言特性太强大。(这块的东西参考一个git吧 https://github.com/bowu678/php_bugs)
        包括不限于 
        - 弱比较==
        - strpos()
        - intval()
        - preg_replace /e问题
        - extract变量覆盖
        - 当然了 变量覆盖的点还有 $$ 等
        - 数字开头字符串和数字比较。
        - 00截断(这里我列的肯定是不全，这块我准备慢慢更新吧)截断应该在5.3之前把。
        - php命令注入怎么防御
        - escapeshellcmd和escapeshellargs2个函数一起使用会造成什么安全问题。
    - thinkphp SQL注入的分析过程(3.2版本中的find(),delete(),select()分析一下这几个函数，跟踪一下)(我分析的一处 https://tiaotiaolong.net/2019/07/19/Thinkphp3.2-SQL注入分析/ )收录到我的git项目 [tiaoVulenv](https://github.com/tiaotiaolong/tiaoVulenv) 中

    - php fpm未授权访问
    - php-fpm跟nginx搭配的情况下，可以通过nginx的特殊配置造成代码执行。
    - thinkphp 命令执行的分析过程(5.x的命令执行) 
    - php的反序列化漏洞，和序列化中的那几个魔法函数。unserialize()
    - webshell变形(可以利用php的特性)，那么问题来了，有什么好的检测方法或者思路可以杜绝任意的php变形webshell？行为检测？还是其他方案。
    - phpinfo解读 从泄露的phpinfo中你能解读写什么东西?(以前渗透测试时候基本都忽略了，下面有篇文章  http://tiandiwuji.top/posts/23527/)
    - Joomla 的反序列化 这个比较经典 涉及到了php构造对象注入。
    - php伪协议
    - typecho反序列化漏洞，这个算是一个老洞了，但是我觉得这个漏洞利用魔法函数触发可以说是较为经典。就算是告诉我这个漏洞点，我也找不到利用链啊。[Typecho反序列化漏洞分析](https://www.anquanke.com/post/id/155306)
    
       
- **Java家族安全**
    - 著名java反序列化漏洞 Apache的common Collection组件里的调用链的原理和利用思路(这个文章特别多) 后续的很多软件的漏洞都是因为使用了这个apache的组件导致的。我写了一个关于我的理解( https://tiaotiaolong.net/2019/07/19/Apache-Common组件反序列化原理/ )同时也收录到我自己的git项目 [tiaoVulenv](https://github.com/tiaotiaolong/tiaoVulenv) 里。

    - 关于java反序列化一般都是怎么修复的，修复思路是什么？黑名单？
    - fastjson 反序列化的问题 关于fastjson我写了一个连载，在博客里，同时也在我自己的git项目[tiaoVulenv](https://github.com/tiaotiaolong/tiaoVulenv)里。
    - shiro 认证模块反序列化漏洞 大致原理以及利用方式。shiro在自己并不是采用class.forname()的方式进行加载的，导致无法支持数组类型的装载，在调用链上有依赖性。
    - shiro的密码学安全问题，PaddingOracle安全问题。
    - Spring 安全 jndi注入和其他好多次的SpEL表达式注入，针对表达式注入有什么好的思路修复吗。
    - Struts2 安全 我觉得和spring表达式安全问题差不多，逐渐被淘汰，可以先去理解SpEL。
    - JBoss 安全 
    - Tomcat 安全 put类型文件上传 和 最近新出的GhostCat
    - WebLogic安全 原理 利用方法  本质问题是java的xmldecode的反序列化问题。这个调试起来比较有难度！
    - jenkins 安全问题
    - ysoserial你真的会了吗？里面几十种调用链，光commoncollection系列目前就7个，可以好好的用idea调试一下ysoserial，你会发现ysoserial的调用链太精彩了。
    - JVM学习 可以参考深入理解Java虚拟机。学习JVM主要对我们理解类加载器装载类的过程有很大帮助，对反序列化的理解的帮助是巨大的。
    - java动态代理，反射，spring的IOC。
    - springboot快速创建一个简单的增删改查项目，使用maven构建，有助于我们复现漏洞环境，总不能依赖于docker吧。
    - SSM框架可以不会写，但是要会看懂代码之间的逻辑运行关系，会读懂每个xml文件，便于审计，开发的话可以直接使用springboot。
    - 利用RMI JNDI注入来完成命令执行的模式是怎样的，了解一下RMI协议，说说rmi的调用过程。
    - JDK7u21 调用链 https://tiaotiaolong.net/2020/03/15/JDK7U21调用链/
    - RMI和LDAP攻击在java的高版本是有防御机制的，那如何绕过该防御机制。
    
    
    
- **企业安全相关**
    - Redis主从命令执行攻击的原理。
        
    
  
- **Python**
    - python参数传递是依靠值传递还是引用传递？
        - 传入可变对象和传入不可变对象的结果一样吗？ 为什么
    - python lambda表达式
    - python 闭包
    - python 装饰器
    

- **应急响应 or 红蓝对抗**
    - php扩展门 
    - pwnginx后门 如果机器存在这种门，该怎么发现它？
    - 自己纯手动搭建一次nginx,apache,tomcat。做到了解所有目录结构和配置文件。
    - apache的扩展后门都有哪些？可以自己动手搭建一下，那这种后门的缺点是什么？
    
 
    
- **安全开发**    
    - 利用openresty写一个简易版本的WAF。谈谈基于规则检测恶意流量的缺点和优点，那如果是基于算法呢？
    - 利用Celery实现自己的扫描器，说说扫描器的思路。
    - 如何设计一款白盒扫描器，可以定位到漏洞点和追踪数据流向。
    - 如果让你设计一个简单的web框架，你如何设计。
    
    
- **开放问题** 
    - XXE跟SSRF你觉得有什么关系吗，相同点跟不同点都可以说说。
    
    
        
    
    
            
    
    


