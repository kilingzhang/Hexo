---
title: hexo 置顶问题
date: 2017-08-22 01:36:10
tags: [hexo]
---

一个月前换了个新的本本，把本本配置好后同步了一下博客的内容。直到今天我才发现，我的置顶功能没有了！！！！！好吧。忘记了换了电脑后没有重新安装依赖。今天记录下hexo的置顶配置，以防以后再出现类似情况后忘记如何操作。

<!--more-->

## 解决问题喽

1. 先安装`hexo-generator-index`
>打开Hexo的根目录
```
npm i --save hexo-generator-index
```
>如果安装了`cnpm`的你懂的~

2. 编辑`node_modules/hexo-generator-index/lib/generator.js`

```
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals) {
    var config = this.config;
    var posts = locals.posts.sort(config.index_generator.order_by);
    var paginationDir = config.pagination_dir || 'page';
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
    return pagination('', posts, {
        perPage: config.index_generator.per_page,
        layout: ['index', 'archive'],
        format: paginationDir + '/%d/',
        data: {
          __index: true
        }
    });
};

```
3. 使用(top值越大排在越前面)

```
---
title: hexo 置顶问题
date: 2017-08-22 01:36:10
tags: [hexo]
top: 100000
---

```


## 参考
[hexo置顶功能](http://spxiaomin.github.io/2017/02/12/hexo%E7%BD%AE%E9%A1%B6%E5%8A%9F%E8%83%BD/)