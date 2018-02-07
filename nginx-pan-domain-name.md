---
title: Nginx进行泛域名解析 
date: 2018-02-07 17:55:52
tags: nginx
---
### 主要目的：
在微信中做H5游戏页面，为了防止微信封域名导致网站全部H5都被禁。

### 主要实现：
    xxx.domain2.domain1.com -> domain2.domain1.com/xxx
    xxx.domain2.domain1.com -> domain2.domain1.com/?id=xxx

nginx rewrite实现二级或三级域名泛解析
在nginx的相对应的配置文件的server中添加
```python
if ($host ~* ^([^\.]+)\.([^\.]+)\.([^\.]+)\.([^\.]+)$) {  #三级域名
    set $subdomain $1;
    set $domain $2;
}

location / { #添加rewrite
    rewrite ^/(.*)$ /index.html/$1$subdomain last;
    break;
}
```
