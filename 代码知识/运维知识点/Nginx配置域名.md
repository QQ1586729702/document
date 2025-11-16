# Nginx配置静态和变量域名区别

### 配置详情

```nginx
server {
   listen       10005;
   server_name  www.exmalp.com;
   resolver 223.5.5.5 8.8.8.8 ipv6=off ipv4=on;
   set $upstream_target "https://www.baidu.com";

   location / {
       proxy_pass $upstream_target;
    }
}

server {
   listen       10006;
   server_name  www.exmalp.com;
   resolver 223.5.5.5 8.8.8.8 ipv6=off ipv4=on;

   location / {
       proxy_pass https://www.baidu.com;
    }
}
```

### 配置区别

`1005`端口使用**变量**来配置代理域名而`1006`用**静态域名**

1. 如果使用变量，会在解析域名时走`resolver`的配置（不用担心效率问题，nginx本身会缓存DNS解析结果，默认5min，可通过`valid`参数配置）。
2. 如果使用**静态域名**会第一次nginx加载时解析一次(系统中的DNS配置)后，缓存起来，直到下次重载或者重启。
