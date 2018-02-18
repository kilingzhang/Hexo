---
title: Web QQ协议分析(二)登陆
tags:
  - WebQQ协议
  - QQ机器人
  - PHP
id: 129
categories:
  - Python
  - QQRobot
  - SmartQQ
  - 后端
date: 2016-08-04 17:47:09
---

原谅部分文字手懒复制的前辈文字：语法部分改为`Python`
目前版本的Web QQ协议，整个登录流程包含以下五步：
<!--more-->
1\. 获取二维码
2\. 确认二维码已被扫描
3\. 获取鉴权参数`ptwebqq`
4\. 获取鉴权参数`vfwebqq`
5\. 获取鉴权参数`uin`和`psessionid`

登录的目的是获得以下五个参数，用于之后请求其它接口：

1.  ptwebqq：保存在Cookie中的鉴权信息
2.  vfwebqq：类似于Token的鉴权信息
3.  psessionid：类似于SessionId的鉴权信息
4.  clientid：设备id，为固定值53999199
5.  uin：登录用户id（其实就是当前登录的QQ号）

流程一：获取二维码
请求方式：`Get`
url：`https://ssl.ptlogin2.qq.com/ptqrshow`
参数:

        params = {
            'appid' : 501004106,
            'e' : 0,
            'l' : 'M',
            's' : 5,
            'd' : 72,
            'v' : 4,
            't' : time.time()
        }
    `</pre>

    返回内容：二维码图片（PNG格式）

    `
     public static function hash33($t) {
            for ( $e = 0, $i = 0, $n = strlen($t); $n > $i; ++$i){
                $e += ($e &lt;&lt; 5) + self::charCodeAt($t,$i);
            }
            return 2147483647 &amp; $e;
        }

    <pre><code>public static function charCodeAt($str, $index)
    {
        $char = mb_substr($str, $index, 1, 'UTF-8');

        if (mb_check_encoding($char, 'UTF-8'))
        {
            $ret = mb_convert_encoding($char, 'UTF-32BE', 'UTF-8');
            return hexdec(bin2hex($ret));
        }
        else
        {
            return null;
        }
    }
    $ptqrtoken = hash33(substr($cookie['qrsig'],6,strlen($cookie['qrsig'])-7))
    `</pre>

    </code>

    流程二：获取二维码扫描状态
    请求方式：`Get`
    url:`https://ssl.ptlogin2.qq.com/ptqrlogin?ptqrtoken={$ptqrtoken}&webqq_type=10&amp;remember_uin=1&amp;login2qq=1&amp;aid=501004106&amp;u1=http%3A%2F%2Fw.qq.com%2Fproxy.html%3Flogin2qq%3D1%26webqq_type%3D10&amp;ptredirect=0&amp;ptlang=2052&amp;daid=164&amp;from_ui=1&amp;pttype=1&amp;dumy=&amp;fp=loginerroralert&amp;action=0-0-4190&amp;mibao_css=m_webqq&amp;t=undefined&amp;g=1&amp;js_type=0&amp;js_ver=10151&amp;login_sig=&amp;pt_randsalt=0`

    <pre>`headers = {
            'Referer':'https://ui.ptlogin2.qq.com/cgi-bin/login?daid=164&amp;target=self&amp;style=16&amp;mibao_css=m_webqq&amp;appid=501004106&amp;enable_qlogin=0&amp;no_verifyimg=1 &amp;s_url=http%3A%2F%2Fw.qq.com%2Fproxy.html&amp;f_url=loginerroralert &amp;strong_login=1&amp;login_state=10&amp;t=20131024001',
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2774.3 Safari/537.36'

        }
    `</pre>

    返回内容：

    扫描前（未失效）：

    <pre>`ptuiCB('66','0','','0','二维码未失效。（3203423232）','');  
    `</pre>

    扫描前（已失效）：

    <pre>`
    ptuiCB('65','0','','0','二维码已失效。(4012918406)', ''); 
    `</pre>

    扫描后，认证前：

    <pre>`
    ptuiCB('66','0','','0','二维码认证中。（3203423232）','');
    `</pre>

    认证后：

    <pre>`ptuiCB('66','0','','0','http://ptlogin4.web2.qq.com/check_sig?xxxxxx','');
    `</pre>

    这个请求可以直接轮训请求，直到认证成功后，将返回的地址保存下来用作下次请求。

    流程三：获取`ptwebqq`
    请求方式：Get

    url：刚才获得的地址(`http://ptlogin4.web2.qq.com/check_sig?xxxxxx`)

    referer：`http://s.web2.qq.com/proxy.html?v=20130916001&amp;callback=1&amp;id=1`

    这个请求成功后的Http状态码是`302`，之后需要将Cookie中的`ptwebqq`保存下来。

    流程四：获取`vfwebqq`
    请求方式：`Get`

    url:`http://s.web2.qq.com/api/getvfwebqq`

    <pre>`headers = {
            'Referer':'http://s.web2.qq.com/proxy.html?v=20130916001&amp;callback=1&amp;id=1',
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2774.3 Safari/537.36'

        }
    `</pre>

    <pre>` params= {
        'ptwebqq':ptwebqq ,
        'clientid':'53999199',
        'psessionid': '',
        't':time.time()
        }
    `</pre>

    url中需要填入上一步获取到的`ptwebqq`，请求成功后会返回一个`JSON`，将`reponse.json()['result']['vfwebqq']`保存下来。

    流程五：获取`psessionid`和`uin`

    请求方式：`Post`

    url:`http://d1.web2.qq.com/channel/login2`

    <pre>`    headers = {

            'Referer': 'http://d1.web2.qq.com/proxy.html?v=20151105001&amp;callback=1&amp;id=2',
            'Origin' :'http://d1.web2.qq.com',
            'User-Agent': 'Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.101 Safari/537.36'
        }
    `</pre>

    <pre>`data = {
            "r" : json.dumps({
          "ptwebqq": ptwebqq,
          "clientid": 53999199,
          "psessionid": "",
          "status": "online",
        }) ,
        }

其中真正的动态参数只有`ptwebqq`，请求成功后会返回一个JSON，将`reponse.json()['result']['psessionid']`和`reponse.json()['result']['uin']`保存下来。

需要注意的是，这里也返回了一个`reponse.json()['result']['vfwebqq']`，但是这个值是无用的。

虽然说按照这个流程可以爬出所有的数据，但是我在自己测试的时候发现了许多不一样的结果，比如`uin`其实在第二步的时候就可以获取，`ptwebqq`也并不是只有第三步才是第一次出现。所以具体看各位的爱好了。不过这个获取的流程腾讯一直在变，既然我觉得写三翼的机器人，那么这个项目我也会一只维护下去。所以这个博客会时时更新的。也欢迎大家提醒或帮助我更新。