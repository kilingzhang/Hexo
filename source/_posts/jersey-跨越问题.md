---
title: jersey  跨越问题
date: 2017-09-28 01:17:29
tags: [jersey , java]
---

引言：
> 最近在写失物招领，之前一直是用 `php` 写后端，所以这个项目就准备用`java`来实现。本来是看[@qiujuer](https://github.com/qiujuer) 老师的[IM](http://coding.imooc.com/lesson/100.html)的安卓课程。最后却用老师讲的java来写后端了。hhhhh。额，跑题了。。。。 

> 所以也就出现了本文的 `jersey` 框架跨域问题。在前端使用`axios`调用接口的时候，发生了跨域的错误提示，如果是`php`，那很简单，在接口文件中添加`header("Access-Control-Allow-Origin:*")`或者改`nginx`的配置文件,但是jav却使用了更强大的--过滤器方法解决。废话不多说，直接说解决方案。  

## 创建`CrossFilter.java`

```
import javax.ws.rs.container.ContainerRequestContext;
import javax.ws.rs.container.ContainerResponseContext;
import javax.ws.rs.container.ContainerResponseFilter;
import java.io.IOException;

public class CrossFilter implements ContainerResponseFilter {
    @Override
    public void filter(ContainerRequestContext requestContext, ContainerResponseContext responseContext) throws IOException {
        responseContext.getHeaders().add("Access-Control-Allow-Origin", "*");
        responseContext.getHeaders().add("Access-Control-Allow-Headers", "token , x-requested-with, Content-Type,Origin, Accept, authorization");
        responseContext.getHeaders().add("Access-Control-Allow-Credentials", "false");
        responseContext.getHeaders().add("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS, HEAD");
        responseContext.getHeaders().add("Access-Control-Max-Age", "1209600");
    }
}

```


## 在`Application.java`中注册此过滤器

```

public class Application extends ResourceConfig {
    public Application(){
        // 注册逻辑处理的包名
        packages(AccountService.class.getPackage().getName());
        // 注册我们的全局请求拦截器
        register(AuthRequestFilter.class);
        // 注册Json解析器
        register(GsonProvider.class);




        
        //注册过滤器
        register(CrossFilter.class);




        // 注册日志打印输出
        register(Logger.class);

    }
}



```


## 运行 大功告成

> 添加后，`axios` 跨域请求再也不会出现跨域错误了，但是，由于`axios`自身机制的问题，又出现了新的问题。这个坑就留作日后来记录吧~~