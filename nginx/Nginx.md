# 基础

## 什么是Nginx？

Nginx 是一个高性能的HTTP和反向代理服务器，并发能力强，能经受高负载的考验，能支持高达50,000个并发连接数。



## 反向代理

> 正向代理（梯子）
>
> ​		用户直接访问谷歌会404，通过一个服务器做中间的代理访问谷歌称为正向代理。

​		反向代理客户端对代理是无感的，因为客户端不需要任何配置就可以访问。只需要将请求发送到反向代理服务器，反向代理服务器去选择目标服务器获取数据后，再返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器的地址，隐藏了真实的服务器ip地址。







## 负载均衡

​		代理服务器通过**轮询**将请求平均分发到目标服务器。









## 动静分离

​		将动态资源（ jsp 、servlet ） 与静态资源（ html 、 css 、 js）分离开。

​		Nginx中存放静态资源，服务端Tomcat存放动态资源。





# 配置

## 开放防火墙端口

```bash
# 添加规则
sudo firewall-cmd-add-port=80/tcp --permanent

# 查看开放的端口号
firewall-cmd --list-all

# 重启防火墙
firewall-cmd-reload
```



## 常用命令

```nginx
# 查看版本
./nginx -v
# 启动
./nginx
# 关闭
./nginx -s stop
# 重新加载配置
./nginx -s reload
```





## 配置文件组成

### 全局块

```nginx
worker_processes 值越大，支持的并发数越多
```





### events块

​		主要影响Nginx服务器与用户的网络连接，常用的设置包括是否开启对多`work process`下的网络连接进行序列化，是否允许同时接收多个网络连接，选取哪种事件驱动模型来处理连接请求，每个`work process`可以同时支持的最大连接数等。

```nginx
worker_connections 支持的最大连接数
```





### http块

#### http全局块

```nginx
# 配置负载均衡
http{
    upstream mystream{
        server 192.168.1.1:8080;
        server 192.168.1.1:8081;
    }
    
    server{
        listen 80;
        server_name 192.168.1.1;
        
        location / {
            proxy_pass http://mystream;
        }
    }
}
```









#### server块

```nginx
server{
    listen 80;  #监听端口
    server_name 192.168.1.1; #暴露的代理服务器地址
    
    location / {
        root html;
        proxy_pass http://127.0.0.1:8080;  #转发到8080端口
        index index.html;
    }
}


# location 的相关正则表达式
# `=` 不使用正则，要求请求字符串与uri严格匹配，如果匹配成功，就停止继续向下搜搜并立即处理该请求
# `~` 区分大小写的正则表达式
# `~*` 不区分大小写的正则表达式
# ·^~· 不包含正则表达式，要求Nginx服务器找到标识uri和请求字符串匹配度最高的location后，立即使用
#      此location处理请求，而不再使用location块中的正则uri和请求字符串做匹配
server{
    listen 80;
    server_name 192.168.1.2;
    
    location ~ /edu/ {
        proxy_pass http://127.0.0.1:8081;  #使用正则，转发~/edu/*到8081端口
    }
    location ~ /vod/ { 
        proxy_pass http://127.0.0.1:8082; 
    }
}
```





## 负载均衡的分配策略

### 轮询

​		每个请求按时间顺序逐一分配到不同的后端服务器，如果某一个后端服务器宕机，自动剔除出队列。



### 权重 weight

​		默认的权重为**1** ，权重越高，分配的几率越大



### ip_hash

​		每个请求按访问ip的hash结果分配，这样每个访客固定访问一个服务器，可以解决session问题

```nginx
upstream mystream{
    ip_hash
        server ...:8081;
        server ...:8082;
}
```





### fair 

​		按后端服务器的响应时间来分配请求，响应时间短的优先分配

```nginx
upstream mystream{
    server ...:8081;
    server ...:8082;
    fair
}
```



## 动静分离

​		通过`location`指定不同的后缀名实现不同的请求转发。通过`expires`参数设置，可以使浏览器缓存过期时间，减少与服务器请求的流量。

​		`expires`给一个资源设定过期时间，过期时间内无需去与服务端之间进行验证，直接通过浏览器自身是否过期即可，所以不会产生额外的流量。`3d`表示三天内访问这个url，发送一个比对请求去与服务器比对该文件最后更新时间是否发生变化，如果没有变化返回状态码`304`，如果发生变化返回状态码`200`

```nginx
server{
    listen 80;
    server_name 192.168.1.1;
    
    location /www/ {
        root /data/;
    }
    
    location /image/ {
        root /data/;
        autoindex on; #列出当前文件夹
    }
}
```









<div style="height:100px;display:flex;justify-content:center;align-items:center;background-image: linear-gradient( 135deg, #81FBB8 10%, #28C76F 100%);font-size:3em;border-radius:8px;box-shadow:0 0 16px 5px rgba(0,0,0,.05);">示例</div>

```nginx
location = / {
 # 精确匹配 / ，主机名后面不能带任何字符串
 [ configuration A ]
}

location / {
 # 因为所有的地址都以 / 开头，所以这条规则将匹配到所有请求
 # 但是正则和最长字符串会优先匹配
 [ configuration B ]
}

location /documents/ {
 # 匹配任何以 /documents/ 开头的地址，匹配符合以后，还要继续往下搜索
 # 只有后面的正则表达式没有匹配到时，这一条才会采用这一条
 [ configuration C ]
}

location ~ /documents/Abc {
 # 匹配任何以 /documents/Abc 开头的地址，匹配符合以后，还要继续往下搜索
 # 只有后面的正则表达式没有匹配到时，这一条才会采用这一条
 [ configuration CC ]
}

location ^~ /images/ {
 # 匹配任何以 /images/ 开头的地址，匹配符合以后，停止往下搜索正则，采用这一条。
 [ configuration D ]
}

location ~* \.(gif|jpg|jpeg)$ {
 # 匹配所有以 gif,jpg或jpeg 结尾的请求
 # 然而，所有请求 /images/ 下的图片会被 config D 处理，因为 ^~ 到达不了这一条正则
 [ configuration E ]
}

location /images/ {
 # 字符匹配到 /images/，继续往下，会发现 ^~ 存在
 [ configuration F ]
}

location /images/abc {
 # 最长字符匹配到 /images/abc，继续往下，会发现 ^~ 存在
 # F与G的放置顺序是没有关系的
 [ configuration G ]
}

location ~ /images/abc/ {
 # 只有去掉 config D 才有效：先最长匹配 config G 开头的地址，继续往下搜索，匹配到这一条正则，采用
  [ configuration H ]
}

location ~* /js/.*/\.js
```



[nginx配置location总结location正则写法及rewrite规则写法]:"https://www.jb51.net/article/148809.htm"