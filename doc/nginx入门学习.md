# Nginx入门学习

## 传送门

* [Nginx官方网站](http://nginx.org)
* [PCRE Library](http://sourceforge.net/projects/pcre/files/pcre/)

## 如何安装

1. [下载源码包](http://nginx.org)
2. 压缩源码包 `tar -xf nginx-1.7.10.tar.gz`
3. 安装依赖库 `brew install pcre` 
4. 安装源码包 `./configure`，`make`，`make install`，`make clean`

## 如何使用Nginx

默认生成路径为`/usr/local/nginx`

### 如何开启Nginx服务

	./sbin/nginx
	
### 如何配置Nginx

	vim ./conf/nginx.conf
	
`nginx.conf`是Nginx反向代理的配置文件，通过修改这个文件来进行代理服务。

```
server {
        listen       80;
        server_name  localhost:3000;
        location / {
				root html;
				index.html index.htm;
        }
    }
```

服务配置项在server这个JSON对象中，下面来简单的看一下这些配置项

| 字段 | 介绍信息 |
|------|--------|
| listen      | 当前的代理服务器监听的端口，默认为80。`注意，若配置了多个server，这个listen要配置不一样，不然就不能确定转到哪里去了。`|
| server_name |监听到之后需要转到哪里去，这里直接转到本地，且到nginx文件夹内|
| location    |匹配的路径，这时配置了/表示所有请求都被匹配到这里|
| root        |当匹配这个请求的路径时，将会在这个文件夹内寻找相应的文件，这里对我们之后的静态文件伺服很有用|
| index       |当没有指定主页时，默认会选择这个指定的文件，它可以有多个，并按顺序来加载，如果第一个不存在，则找第二个，依此类推。|

### 

### 拦截

location 后面可以配置正则表达式，可以将不同的文件，传达给不同的服务。

例如：

```
// jsp
location ~ \.jsp$ {  
        proxy_pass http://localhost:8080;  
}  
  
// 或者可以这样
location ~ \.(html|js|css|png|gif)$ {  
    root D:/software/developerTools/server/apache-tomcat-7.0.8/webapps/ROOT;  
}  
```

### 如何设置转发多台服务器

具体配置参照下面的JSON：

```
server {
        location / {
             proxy_pass http://local_express;
        }
    }
upstream local_express {  
    server localhost:3000 weight=1;  
    server localhost:3001 weight=5; 
} 
```

**注意：** `weight`的数值越大，请求到的几率就越大。
