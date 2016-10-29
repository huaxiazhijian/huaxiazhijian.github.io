---
layout:     post
title:      "Session&Cookie「原」"
subtitle:   "Session&Cookie"
date:       2016-10-29
author:     "LingDie | 靈蝶"
header-img: "img/blog/20161029Session.jpg"
tags:
    - 存储
    - Session
    - Cookie
    - 原创
---

> 下滑这里查看更多内容

### Session
Session:在计算机中，尤其是在网络应用中，称为“会话控制”。Session 对象存储特定用户会话所需的属性及配置信息。这样，当用户在应用程序的 Web 页之间跳转时，存储在 Session 对象中的变量将不会丢失，而是在整个用户会话中一直存在下去。当用户请求来自应用程序的 Web 页时，如果该用户还没有会话，则 Web 服务器将自动创建一个 Session 对象。当会话过期或被放弃后，服务器将终止该会话。Session 对象最常见的一个用法就是存储用户的首选项。例如，如果用户指明不喜欢查看图形，就可以将该信息存储在 Session 对象中。有关使用 Session 对象的详细信息，请参阅“ASP 应用程序”部分的“管理会话”。注意 会话状态仅在支持 cookie 的浏览器中保留。
- 工作原理
（1）当一个session第一次被启用时，一个唯一的标识被存储于本地的cookie中。
（2）首先使用session_start()函数，PHP从session仓库中加载已经存储的session变量。
（3）当执行PHP脚本时，通过使用session_register()函数注册session变量。
（4）当PHP脚本执行结束时，未被销毁的session变量会被自动保存在本地一定路径下的session库中，这个路径可以通过php.ini文件中的session.save_path指定，下次浏览网页时可以加载使用。
- PHP使用Session
原生态php的session简单使用如下：
sesstion_start();                // 首先开启session
$_SESSION['user'] = 'username';  // 把username存在$_SESSION['user'] 里面
echo $_SESSION['user'];          // 直接输出 username

session_destroy();               // 销毁session

### Cookie
Cookie 是在 HTTP 协议下，服务器或脚本可以维护客户工作站上信息的一种方式。Cookie 是由 Web 服务器保存在用户浏览器（客户端）上的小文本文件，它可以包含有关用户的信息。无论何时用户链接到服务器，Web 站点都可以访问 Cookie 信息 。
目前有些 Cookie 是临时的，有些则是持续的。临时的 Cookie 只在浏览器上保存一段规定的时间，一旦超过规定的时间，该 Cookie 就会被系统清除。
持续的 Cookie 则保存在用户的 Cookie 文件中，下一次用户返回时，仍然可以对它进行调用。在 Cookie 文件中保存 Cookie，有些用户担心 Cookie 中的用户信息被一些别有用心的人窃取，而造成一定的损害。其实，网站以外的用户无法跨过网站来获得 Cookie 信息。如果因为这种担心而屏蔽 Cookie，肯定会因此拒绝访问许多站点页面。因为，当今有许多 Web 站点开发人员使用 Cookie 技术，例如 Session 对象的使用就离不开 Cookie 的支持。
- 工作原理
要了解Cookie，必不可少地要知道它的工作原理。一般来说，Cookie通过HTTP Headers从服务器端返回到浏览器上。
首先，服务器端在响应中利用Set-Cookie header来创建一个Cookie ，然后，浏览器在它的请求中通过Cookie header包含这个已经创建的Cookie，并且把它返回至服务器，从而完成浏览器的论证。
例如，我们创建了一个名字为login的Cookie来包含访问者的信息，创建Cookie时，服务器端的Header如下面所示，这里假设访问者的注册名是“Michael Jordan”，同时还对所创建的Cookie的属性如pathdomain、expires等进行了指定。
expires=Monday,01-Mar-99 00:00:01 GMT
上面这个Header会自动在浏览器端计算机的Cookie文件中添加一条记录。浏览器将变量名为“login”的Cookie赋值为“Michael Jordon”。注意，在实际传递过程中这个Cookie的值是经过了URLEncode方法的URL编码操作的。这个含有Cookie值的HTTP Header被保存到浏览器的Cookie文件后，Header就通知浏览器将Cookie通过请求以忽略路径的方式返回到服务器，完成浏览器的认证操作。
此外，我们使用了Cookie的一些属性来限定该Cookie的使用。例如Domain属性能够在浏览器端对Cookie发送进行限定，具体到上面的例子，该Cookie只能传送到指定的服务器上，而决不会跑到其他的Web站点上去。Expires属性则指定了该Cookie保存的时间期限，例如上面的Cookie在浏览器上只保存到1999年3月1日1秒。当然，如果浏览器上Cookie 太多，超过了系统所允许的范围，浏览器将自动对它进行删除。至于属性Path，用来指定Cookie将被发送到服务器的哪一个目录路径下。
说明：浏览器创建了一个Cookie后，对于每一个针对该网站的请求，都会在Header中带着这个Cookie；不过，对于其他网站的请求Cookie是绝对不会跟着发送的。而且浏览器会这样一直发送，直到Cookie过期为止。
- PHP使用Cookie
setcookie(name, value, expire, path, domain, secure)    
name 必需。规定 cookie 的名称。
value 必需。规定 cookie 的值。
expire 可选。规定 cookie 的有效期。
path 可选。规定 cookie 的服务器路径。
domain 可选。规定 cookie 的域名。
secure 可选。规定是否通过安全的 HTTPS 连接来传输 cookie。
可以通过 $HTTP_COOKIE_VARS["user"] 或 $_COOKIE["user"] 来访问名为 "user" 的 cookie 的值。在发送 cookie 时，cookie 的值会自动进行 URL 编码。接收时会进行 URL 解码。如果不需要这样，可以使用 setrawcookie() 代替。
```

	    <?php    
		    $value = "my cookie value";    
		    // 发送一个简单的 cookie    
		    setcookie("TestCookie",$value);    
	    ?>    
	    <?php    
		    $value = "my cookie value";    
		    // 发送一个 24 小时候过期的 cookie    
		    setcookie("TestCookie",$value, time()+3600*24);    
	    ?>    
	    
```
### Session&Cookie区别
1.cookie 是一种发送到客户浏览器的文本串句柄，并保存在客户机硬盘上，可以用来在某个WEB站点会话间持久的保持数据。
2.session其实指的就是访问者从到达某个特定主页到离开为止的那段时间。 Session其实是利用Cookie进行信息处理的，当用户首先进行了请求后，服务端就在用户浏览器上创建了一个Cookie，当这个Session结束时，其实就是意味着这个Cookie就过期了。
注：为这个用户创建的Cookie的名称是aspsessionid。这个Cookie的唯一目的就是为每一个用户提供不同的身份认证。
3.cookie和session的共同之处在于：cookie和session都是用来跟踪浏览器用户身份的会话方式。
4.cookie 和session的区别是：cookie数据保存在客户端，session数据保存在服务器端。
5.两个都可以用来存私密的东西，同样也都有有效期的说法,区别在于session是放在服务器上的，过期与否取决于服务期的设定，cookie是存在客户端的，过去与否可以在cookie生成的时候设置进去。
(1)cookie数据存放在客户的浏览器上，session数据放在服务器上
(2)cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗,如果主要考虑到安全应当使用session
(3)session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能，如果主要考虑到减轻服务器性能方面，应当使用COOKIE
(4)单个cookie在客户端的限制是3K，就是说一个站点在客户端存放的COOKIE不能3K。
(5)所以：将登陆信息等重要信息存放为SESSION;其他信息如果需要保留，可以放在COOKIE中
