---
title: hexo 添加CNZZ站长统计
date: 2017-04-30 23:57:02
tags: [hexo]
---

前几天弄好我的hexo博客后,把之前的wordpress的文章迁移后,今天我继续完善我的hexo博客,准备添加站长统计的功能,首先去CNZZ去注册一个账号.     
[CNZZ](https://www.umeng.com/ "CNZZ")
![](http://markdown-1252847423.file.myqcloud.com/AT07~%24~C0N%28GB2%40B1NX2KUD.png)       

<!--more-->
![](http://markdown-1252847423.file.myqcloud.com/%40PBYW0O9SZ%5D2Q%7B%7B%299US%29%7D6D.png)     

注册好后,选择web统计产品.

![](http://markdown-1252847423.file.myqcloud.com/%5BNG%244Q8N%5DC7MC1%7BO%7DHKHVM6.png)

![](http://markdown-1252847423.file.myqcloud.com/7JP%24X%25_4OO%29OF%2842%28B%28JQ%28I.png)

![](http://markdown-1252847423.file.myqcloud.com/%7D3%40N_RL_%7BWW_%24%7BUP1L%24L%7BU2.png)

![](http://markdown-1252847423.file.myqcloud.com/V%5DT%60ZC04TI%7D450Y6O0KE%25ZC.png)

选择一个你想要的模式,我选择的是 `横排数据显示`


	<script type="text/javascript">var cnzz_protocol = (("https:" == document.location.protocol) ? " https://" : "    
       http://");document.write(unescape("%3Cspan id='cnzz_stat_icon_1261850225'%3E%3C/span%3E%3Cscript src='" +    
     cnzz_protocol + "s95.cnzz.com/z_stat.php%3Fid%3D1261850225%26online%3D1%26show%3Dline' type='text/javascript'%  
	3E%3C/script%3E"));</script>   

由于我的主题是`yilla`     
所以在 `hexo\themes\yilia\_config.yml` 配置中添加上     
     
               

    #Analytics              
    cnzz: true 


然后在`hexo\themes\pacman\layout\_partial\footer.ejs`中在你想添加的位置添加上上次选择的代码 完整代码如下


	<footer id="footer">
	</script>
	  <div class="outer">
	    <div id="footer-info">
	    	<div class="footer-left">
	    		&copy; <%= date(new Date(), 'YYYY') %> <%= config.author || config.title %>
	    	</div>
			<div>
				  	<script type="text/javascript">var cnzz_protocol = (("https:" == document.location.protocol) ? " https://" : "    
				       http://");document.write(unescape("%3Cspan id='cnzz_stat_icon_1261850225'%3E%3C/span%3E%3Cscript src='" +    
				     cnzz_protocol + "s95.cnzz.com/z_stat.php%3Fid%3D1261850225%26online%3D1%26show%3Dline' type='text/javascript'%  
					3E%3C/script%3E"));</script> 
			</div>
	      	<div class="footer-right">
	      		<a href="http://hexo.io/" target="_blank">Hexo</a>  Theme <a href="https://github.com/litten/hexo-theme-yilia" target="_blank">Yilia</a> by Litten
	      	</div>
	    </div>
	  </div>
	  
	</footer>



然后在命令中输入命令:

		hexo g
		hexo d

最后大功告成~!