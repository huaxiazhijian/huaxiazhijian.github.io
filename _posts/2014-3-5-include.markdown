---
layout:     post
title:      "PHP中require和include的区别 「转」"
subtitle:   "The difference between the require in the PHP and include "
date:       2014-3-5
author:     "LingDie | 靈蝶"
header-img: "img/blog/2014/include.jpg"
tags:
    - PHP
    - include
    - require
    - 转载
---

> 下滑这里查看更多内容

在PHP变成中，include()与require()的功能相同，include（include_once） 与 require（require_once）都是把把包含的文件代码读入到指定位置来，但是二者再用法上有区别：（include()是有条件包含函数，而require()则是无条件包含函数）

1, 使用方式不同

(1) require 的使用方法如 require("requireFile.php"); 。这个函式通常放在 PHP 程式的最前面，PHP 程式在执行前，就会先读入 require 所指定引入的档案，使它变成 PHP 程式网页的一部份。常用的函式，亦可以这个方法将它引入网页中。引入是无条件的，发生在程序执行前，不管条件是否成立都要导入（可能不执行）。
(2) include 使用方法如 include("includeFile.php"); 。这个函式一般是放在流程控制的处理区段中。PHP 程式网页在读到 include 的档案时，才将它读进来。这种方式，可以把程式执行时的流程简单化。引入是有条件的，发生在程序执行时，只有条件成立时才导入（可以简化编译生成的代码）。


例如在下面的一个例子中，如果变量$somgthing为真，则将包含文件somefile：

{% highlight ruby %}

if($something){
include("somefile");
}

{% endhighlight %}

但不管$something取何值，下面的代码将把文件somefile包含进文件里：

{% highlight ruby %}

if($something){
require("somefile");
}

{% endhighlight %}

下面的这个有趣的例子充分说明了这两个函数之间的不同。

{% highlight ruby %}

$i = 1;
while ($i < 3) {
require("somefile.$i");
$i++;
}

{% endhighlight %}

在这段代码中，每一次循环的时候，程序都将把同一个文件包含进去。很显然这不是程序员的初衷，从代码中我们可以看出这段代码希望在每次循环时，将不同的文件包含进来。如果要完成这个功能，必须求助函数include()：

{% highlight ruby %}

$i = 1;
while ($i < 3) {
include("somefile.$i");
$i++;
}

{% endhighlight %}

2. 执行时报错方式不同

include和require的区别：include引入文件的时候，如果碰到错误，会给出提示，并继续运行下边的代码，require引入文件的时候，如果碰到错误，会给出提示，并停止运行下边的代码。例如下面例子：

写两个php文件，名字为test1.php  和test2.php，注意相同的目录中，不要存在一个名字是test3.php的文件。
test1.php

{% highlight ruby %}

<?PHP
include  (”test3.php”);
echo  “abc”;
?>

test2.php
<?PHP
require (”test3.php”)
echo  “abc”;
?>

{% endhighlight %}

浏览第一个文件，因为没有找到test999.php文件，我们看到了报错信息，同时，报错信息的下边显示了abc，你看到的可能是类似下边的情况：

{% highlight ruby %}

Warning: include(test3.php) [function.include]: failed to open stream: No such file or directory in D:\WebSite\test.php on line 2

Warning: include() [function.include]: Failed opening ‘test3.php’ for inclusion (include_path=’.;C:\php5\pear’) in D:\WebSite\test.php on line 2
abc （下面的被执行了）

{% endhighlight %}

浏览第二个文件，因为没有找到test3.php文件，我们看到了报错信息，但是，报错信息的下边没有显示abc，你看到的可能是类似下边的情况：

{% highlight ruby %}

Warning: require(test3.php) [function.require]: failed to open stream: No such file or directory in D:\WebSite\test2.php on line 2

Fatal error: require() [function.require]: Failed opening required ‘test3.php’ (include_path=’.;C:\php5\pear’) in D:\WebSite\test.php on line 2

{% endhighlight %}

下面的未被执行，直接结束

总之，include时执行时调用的，是一个过程行为，有条件的，而require是一个预置行为，无条件的。
