---
title: PHP强大的爬虫类Snoopy
date: 2017-05-14 17:15:35
tags: [PHP, 爬虫, 接口, Snoopy]
---

今天我们来介绍一个PHP的强大的爬虫类`Snoopy`.

<!--more-->

#Snoopy


下载地址：https://sourceforge.net/projects/snoopy/files/

##类属性
$host 连接的主机

$port 连接的端口

$proxy_host 使用的代理主机，如果有的话

$proxy_port 使用的代理主机端口，如果有的话

$agent 用户代理伪装 (Snoopy v0.1)

$referer 来路信息，如果有的话

$cookies cookies， 如果有的话

$rawheaders 其他的头信息, 如果有的话

$maxredirs 最大重定向次数， 0=不允许 (5)

$offsiteok whether or not to allow redirects off-site. (true)

$expandlinks 是否将链接都补全为完整地址 (true)

$user 认证用户名, 如果有的话

$pass 认证用户名, 如果有的话

$accept http 接受类型 (image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, /)

$error 哪里报错, 如果有的话

$response_code 从服务器返回的响应代码

$headers 从服务器返回的头信息

$maxlength 最长返回数据长度

$read_timeout 读取操作超时 (requires PHP 4 Beta 4+)，设置为0为没有超时

$timed_out 如果一次读取操作超时了，本属性返回 true (requires PHP 4 Beta 4+)

$maxframes 允许追踪的框架最大数量

$status 抓取的http的状态

$temp_dir 网页服务器能够写入的临时文件目录 (/tmp)

$curl_path cURL binary 的目录, 如果没有cURL binary就设置为 false


##类方法
fetch($uri)

>这是为了抓取网页的内容而使用的方法。$URI参数是被抓取网页的URL地址。抓取的结果被存储在 $this->results 中。如果你正在抓取的是一个框架，Snoopy将会将每个框架追踪后存入数组中，然后存入 $this->results。

fetchtext($URI)

>本方法类似于fetch()，唯一不同的就是本方法会去除HTML标签和其他的无关数据，只返回网页中的文字内容。

fetchform($URI)

>本方法类似于fetch()，唯一不同的就是本方法会去除HTML标签和其他的无关数据，只返回网页中表单内容(form)。

fetchlinks($URI)

>本方法类似于fetch()，唯一不同的就是本方法会去除HTML标签和其他的无关数据，只返回网页中链接(link)。默认情况下，相对链接将自动补全，转换成完整的URL。

submit($URI,$formvars)

>本方法向$URL指定的链接地址发送确认表单。$formvars是一个存储表单参数的数组。

submittext($URI,$formvars)

>本方法类似于submit()，唯一不同的就是本方法会去除HTML标签和其他的无关数据，只返回登陆后网页中的文字内容。

submitlinks($URI)

>本方法类似于submit()，唯一不同的就是本方法会去除HTML标签和其他的无关数据，只返回网页中链接(link)。默认情况下，相对链接将自动补全，转换成完整的URL。


#实战
- 首页
>`http://hqgl.xtu.edu.cn/repairforeground/workerRepairHome_init.t`

- 登陆页
> `http://hqgl.xtu.edu.cn/repairforeground/pcLogin_init.t`

- 登陆接口
> `http://hqgl.xtu.edu.cn/repairforeground/pcLogin_execute.json`

- 个人信息
>`http://hqgl.xtu.edu.cn/repairforeground/userRepairFrom_personinit.t`


###登录


方式:POST
> URL ：`http://hqgl.xtu.edu.cn/repairforeground/pcLogin_execute.json`    
参数:       
- `login_name`     
- `password`



	<?php
	include_once "Lib/Snoopy.class.php";
	$Snoopy = new Snoopy();
	$param = array(
	    'login_name'=>,
	    'password'=>
	);
	$Snoopy->submit('http://hqgl.xtu.edu.cn/repairforeground/pcLogin_execute.json',$param);
	if($Snoopy->status != 200){
	    //todo
	}else{
	    echo $Snoopy->results;
	}

`Response`

	{
	    "success": true,
	    "errorCode": 0,
	    "message": "/workerRepairHome_init.t"
	}


##获取用户数据

方式:GET
> URL ：`http://hqgl.xtu.edu.cn/repairforeground/userRepairFrom_personinit.t`    
参数:       


	
	<?php
	include_once "Lib/Snoopy.class.php";
	
	$Snoopy = new Snoopy();
	$param = array(
	    'login_name'=>,
	    'password'=>
	);
	$Snoopy->submit('http://hqgl.xtu.edu.cn/repairforeground/pcLogin_execute.json',$param);
	if($Snoopy->status != 200){
	    //todo
	}else{
	    $json = $Snoopy->results;
	    $data = json_decode($json,true);
	    if($data['errorCode'] != 0){
	        //todo
	    }else{
	        $Snoopy->setcookies();
	        $Snoopy->fetch('http://hqgl.xtu.edu.cn/repairforeground/userRepairFrom_personinit.t');
	        if($Snoopy->status != 200){
	            //todo
	        }else{
	            echo $Snoopy->results;
	        }
	    }
	}




`Response` 截取

	<table style="margin-left:200px;">
		<thead>
			<tr>
	           <td width="200" style="font-weight: bold;height:3px;" >用户账户</td>
	           <td width="200" id="userName" style="height:3px;text-align: left;text-indent:0.8cm;"></td>
	        </tr>
	        <tr>
	           <td width="200" style="font-weight: bold;height:3px;" >真实姓名</td>
	           <td width="200" id="name" style="height:3px;text-align: left;text-indent:0.8cm;"></td>
	        </tr>
	        <tr>
	          <td width="200" style="font-weight: bold; height:3px;" >联系方式</td>
	          <td width="200" style="height:3px;text-align: left;text-indent:0.8cm;"><input type="text" id="phone" /></td>
	         </tr>
	         <tr>
	          <td colspan="2" style="border:none">
	          	<input id="tj" type="button" class="mr_input_button_l" value="提交" onclick="javascript:updaMobile()"/>
	          	<br/>
					<div id="subtj" style=";border: 0px;display:none">
						<span style="color:#F00">提交失败：系统正忙，请稍后再试。。。</span>
				</div>
	          </td>
	         </tr>
	        <!--  
	        <tr>
	            <td>学号：</td>
	            <td>1234567890</td>
	            <td>身份证号：</td>
	            <td>0987654321</td>
	        </tr>
	        <tr>
	            <td>院系班级：</td>
	            <td colspan="3">商学院&nbsp;/&nbsp;会计专业&nbsp;/&nbsp;1302班</td>
	        </tr>
	        <tr>
	            <td>现住址：</td>
	            <td colspan="3">南苑片区&nbsp;/&nbsp;南苑1栋&nbsp;/&nbsp;202</td>
	        </tr>
	        <tr>
	            <td>联系方式1：</td>
	            <td>13122223333</td>
	            <td>联系方式2：</td>
	            <td>13455556666</td>
	        </tr>
	        -->
	    </thead>
	</table> 


因为本网站的数据渲染是通过ajax实现，所以即使我们登录成功，我们依然不能直接获取我们用户的个人信息。我们需要分析js文件协议，去找到数据渲染的方式。这个需要一些经验和js知识。此文暂时不做介绍。日后会重写抓取`SmartQQ`的协议。到时候会详细讲到一些抓包分析协议的技巧。至此，我们的模拟登陆示例已经完成，其余的就是利用我们获取到Cookies的Snoopy对象去继续爬去其他的网页分析整理数据了。      
是不是觉得Snoopy很简单，更多的功能期待大家一起去发现。不过Snoopy也有一些蜜汁bug，不过只是个别情况会出现。所以大家也不用担心。安全无毒，请放心食用。