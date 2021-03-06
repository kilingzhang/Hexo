---
title: PHP 时间戳格式设置
id: 69
categories:
  - PHP
  - 后端
date: 2016-08-07 18:41:49
tags:
  - PHP
---

1.  获取当前时间方法date()
很简单，这就是获取时间的方法，格式为：date($format, $timestamp)，format为格式、timestamp为时间戳–可填参数。

2.  获取时间戳方法time()、strtotime()
这两个方法，都可以获取php中unix时间戳，time()为直接获取得到，strtotime($time, $now)为将时间格式转为时间戳，$time为必填。

3.  date($format)用法  比如：
<!--more-->

&nbsp;

    echo date('Y-m-d') ，输出结果：2012-03-22
    echo  date('Y-m-d H:i:s')，输出结果：2012-03-22 23:00:00
    echo  date('Y-m-d', time())，输出结果：2012-03-22 23:00:00（结果同上，只是多了一个时间戳参数）（时间戳转换为日期格式的方法）
    echo  date('Y').'年'.date('m').'月'.date('d').'日'，输出结果：2012年3月22日
    a – "am" 或是 "pm"
    A – "AM" 或是 "PM"
    d – 几日，二位数字，若不足二位则前面补零; 如: "01" 至 "31"
    D – 星期几，三个英文字母; 如: "Fri"
    F – 月份，英文全名; 如: "January"
    h – 12 小时制的小时; 如: "01" 至 "12"
    H – 24 小时制的小时; 如: "00" 至 "23"
    g – 12 小时制的小时，不足二位不补零; 如: "1" 至 12"
    G – 24 小时制的小时，不足二位不补零; 如: "0" 至 "23"
    i – 分钟; 如: "00" 至 "59"
    j – 几日，二位数字，若不足二位不补零; 如: "1" 至 "31"
    l – 星期几，英文全名; 如: "Friday"
    m – 月份，二位数字，若不足二位则在前面补零; 如: "01" 至 "12"
    n – 月份，二位数字，若不足二位则不补零; 如: "1" 至 "12"
    M – 月份，三个英文字母; 如: "Jan"
    s – 秒; 如: "00" 至 "59"
    S – 字尾加英文序数，二个英文字母; 如: "th"，"nd"
    t – 指定月份的天数; 如: "28" 至 "31"
    U – 总秒数
    w – 数字型的星期几，如: "0" (星期日) 至 "6" (星期六)
    Y – 年，四位数字; 如: "1999"
    y – 年，二位数字; 如: "99"
    z – 一年中的第几天; 如: "0" 至 "365"
    `</pre>

1.  strtotime($time)用法
    比如：

    <pre>`echo strtotime('2012-03-22′)，输出结果：1332427715（此处结果为随便写的，仅作说明使用）
    echo strtotime(date('Y-d-m'))，输出结果：（结合date()，结果同上）（时间日期转换为时间戳）
    strtotime()还有个很强大的用法，参数可加入对于数字的操作、年月日周英文字符，示例如下：
    echo date('Y-m-d H:i:s',strtotime('+1 day'))，输出结果：2012-03-23 23:30:33（会发现输出明天此时的时间）
    echo date('Y-m-d H:i:s',strtotime('-1 day'))，输出结果：2012-03-21 23:30:33（昨天此时的时间）
    echo date('Y-m-d H:i:s',strtotime('+1 week'))，输出结果：2012-03-29 23:30:33（下个星期此时的时间）
    echo date('Y-m-d H:i:s',strtotime('next Thursday'))，输出结果：2012-03-2900:00:00（下个星期四此时的时间）
    echo date('Y-m-d H:i:s',strtotime('last Thursday'))，输出结果：2012-03-15 00:00:00（上个星期四此时的时间）
    `</pre>

    strtotime()方法可以通过英文文本的控制Unix时间戳的显示，而得到需要的时间日期格式。

1.  php获取当前时间的毫秒数

    php本身没有提供返回毫秒数的函数，但提供了microtime()方法，它会返回一个Array，包含两个元素：一个是秒数、一个是小数表示的毫秒数，我们可以通过此方法获取返回毫秒数，方法如下：

    <pre>`function getMillisecond() {
        list($s1, $s2) = explode(' ', microtime()); return (float)
        sprintf('%.0f', (floatval($s1) + floatval($s2)) * 1000);
    }

1.  获取当前时间相差6小时解决方法
有些朋友，获取的时间与当前系统时间相差6个小时，这是因为时区设置问题，只要将之设为上海时间即可。方法如下：
2.  在php.ini中找到date.timezone，将它的值改成 Asia/Shanghai，即 date.timezone = Asia/Shanghai
3.  在程序开始时添加 date_default_timezone_set('Asia/Shanghai')即可。