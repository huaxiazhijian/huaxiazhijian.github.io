---
layout:     post
title:      "微信支付详解「转」"
subtitle:   "Pay WeChat explanation  "
date:       2016-10-7
author:     "LingDie | 靈蝶"
header-img: "img/blog/2014/wzf.jpg"
tags:
    - 微信开发
    - PHP
    - 微信支付
    - 转载
---

> 下滑这里查看更多内容

一.前期准备：

首先你们公司开通微信支付功能后，会收到一份邮件，里面有账户相关信息，一般有：微信支付商户号，商户平台登录帐号，商户平台登录密码，申请对应的公众号，公众号APPID。

1.下载demo：用上面信息登陆“微信商户平台”，>>>（右上角开发文档）>>>公众号支付>>>sdk下载>>>选php

2.下载证书：账户中心>>>api安全

    将下载的证书中的所有文件解压到demo的cert文件夹中（demo原先自带的要删掉），然后修改demo中lib/WxPay.Config.php中的以下配置

　　const APPID = '邮件中有,即`公众号APPID`';

　　const MCHID = '邮件中有,即`微信支付商户号`';

　　const KEY = 'wxpay.config.php中注释有相关链接';

　　const APPSECRET = '公众平台开发者中心设置,同样注释中有链接';

3.配置好后去微信公众平台,里面有微信支付功能如下图.在开发配置中设置支付目录和测试目录.这里主要是配置测试目录,支付目录可以先不管(我的域名是www.test.com),然后将自己的微信号加入测试白名单.

<img src="../../../../img/blog/2014/weixin/1.png" alt="">
4.再去微信公众平台>>>开发>>>接口权限>>>网页服务的第一项`网页账号`,修改它的值为你自己的域名(仅仅是域名).如图:
<img src="../../../../img/blog/2014/weixin/2.png" alt="">
<img src="../../../../img/blog/2014/weixin/3.png" alt="">

二.demo代码修改(仅仅针对当前的版本):

     1.  修改文件WxPay.Api.php

   将curl_setopt($ch,CURLOPT_SSL_VERIFYPEER,TRUE);

       curl_setopt($ch,CURLOPT_SSL_VERIFYHOST,2);//严格校验
   改为:

       curl_setopt($ch,CURLOPT_SSL_VERIFYPEER,FALSE);

       curl_setopt($ch,CURLOPT_SSL_VERIFYHOST,FALSE);//严格校验2

 

   2.   如果访问jsapi.php时你要用get方式传递参数,那么你要去修改WxPay.JsapiPay.php中的

       $baseUrl = urlencode('http://'.$_SERVER['HTTP_HOST'].$_SERVER['PHP_SELF'].$_SERVER['QUERY_STRING']);

   改为:

       $baseUrl = urlencode('http://'.$_SERVER['HTTP_HOST'].$_SERVER['PHP_SELF'].'?'.$_SERVER['QUERY_STRING']);

   或者在写链接时写两个??传参,例如:http://www.test.com/demo/example/jsapi.php??id=xxx&...

 

 3.删去wxpay.notify.php中的ReplyNotify函数中$this->GetReturn_code(‘参数’) == "SUCCESS"里面的’参数’.

 

三.开始开发,按大概流程讲述:

    1.支付:首先点击支付后,会到jsapi.php文件中去,获取openid,这步我没管,也没出错;

   然后是统一下单,他会设置一大堆参数,如图:
<img src="../../../../img/blog/2014/weixin/4.png" alt="">
 将里面的setNotify_url设置为你的notify.php文件所在的位置.

   其中的setOut_trade_no和setTotal_fee这两个参数是你可以随便填写的(其他参数默认可以).在支付成功后微信服务器会将这两个参数的值返回给你.我是直接将商品订单号码放到setOut_trade_no中传递过去.在这一步,我遇到的问题是,get过来的字符串参数总是放不到setOut_trade_no中,最后我发现传过来的订单字符串被莫名奇妙的加上了单引号.于是我接收到字符串后先用trim函数处理,然后就能放入了.

   此时点击支付,应该可以去支付了.(支付的结果在商户平台中查看)

 

  2.支付成功后的返回:支付完成后,微信服务器会自动请求你的notify.php文件.但是请求进入后直接通过最后一句$notify->Handle(false);跳到了WxPay.notify.php中,然后还调用了很多其他函数,整个过程我只处理了两个函数.

     ①WxPay.api.php中的notify函数,如图:
<img src="../../../../img/blog/2014/weixin/5.png" alt="">
这里面的$xml=$GLOBALS['HTTP_RAW_POST_DATA'];就是支付成功后用户返回给你的一个结果,他是一个xml格式的字符串.调试的时候可以将它输入到文件查看(结果是重复的,因为微信服务器会重复请求多次).$xml输出结果如图:
<img src="../../../../img/blog/2014/weixin/6.png" alt="">
 其中的$out_trade_no就是在支付之前我自己设置的订单号码.接下来我们想办法得到这个订单号,然后我就直接在下面写支付成功后的逻辑了,如改变数据库中的数据等等.

 这个函数内其他的代码我没有管,只是最后一句return call_user_func($callback, $result);我直接改为return true;了.如果使用return call_user_func($callback, $result);就会有多次请求.

 ②.WxPay.api.php中的replyNotify函数,这个函数就是用来阻止微信服务器重复请求你的notify.php文件的,如图
<img src="../../../../img/blog/2014/weixin/7.png" alt="">
 查看完整个handle函数的调用过程后,会发现最后执行的就是这个函数.他的输出结果(echo $xml)就是notify.php文件中最后一句handle函数的结果.$xml的内容是什么呢?

 内容如图:
<img src="../../../../img/blog/2014/weixin/8.png" alt=""> 
 可以看到$xml是一个xml格式字符串,他就是文档中说的返回给微信服务器的内容.所以我们不用做其他额外的返回操作了,微信自己就有返回结果的方法.

 一开始做什么都不懂,全是看别人的文章,在这一步看好多人说直接输出success,return success,或者写入流,等等,都没有成功,总是被请求多次.后来狠心看了代码,发现竟然有现成的.

四.支付后跳转

支付完成后,手机提示支付成功,并会显示支付信息,点击手机上的完成,页面可以跳转到你设置的页面.设置的地方是在jsapi.php文件中的jsApiCall函数中,我直接设置为跳转到百度.你设置为自己想要跳转到的页面路径即可.
<img src="../../../../img/blog/2014/weixin/9.png" alt="">

至此,一个支付流程大致走完了。

 