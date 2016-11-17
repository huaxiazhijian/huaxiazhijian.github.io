---
layout:     post
title:      "设置session生存时间问题  「原」"
subtitle:   "Set up the session survival time "
date:       2014-4-1
author:     "LingDie | 靈蝶"
header-img: "img/blog/2014/session.jpg"
tags:
    - session
    - 原创
---

> 下滑这里查看更多内容

{% highlight ruby %}

     #在 php.ini 中设置 session.gc_maxlifetime = 1440 (默认)
 
 #或者在 session_start() 前，设置 $lifetime = 86400 , 执行 session_set_cookie_params($lifetime), 具体如下
 $lifetime = 86400;
 session_set_cookie_params($lifetime);
 session_start()

{% endhighlight %}