# nginx 常见操作

## 简介

## 代理 websocket 注意事项
一般需要先在nginx.conf里面设置
```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}
```
解释一下map指令的作用：主要是根据客户端请求中的值，来构造改变connection_upgrade的值，即根据变量的值创建新的变量connection_upgrade，
创建的规则就是{}里面的东西。其中的规则没有做匹配，因此使用默认的，即 http_upgrade为空字符串的话，那么值就是 close 。

再到 server 里配置
如：
```
location / {
  # 
  proxy_pass http://backserver;
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  # 下面三行是关键
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "$connection_upgrade";
  # 下面三条是 Nginx代理webSocket经常中断的解决方法（即如何保持长连接），具体值可以视情况调整
  proxy_connect_timeout 5s;
  # 默认值60秒,该指令设置与代理服务器的读超时时间
  proxy_read_timeout 60s;
  # 默认值 60s,设置了发送请求给upstream服务器的超时时间
  proxy_send_timeout 30s;
}
```


