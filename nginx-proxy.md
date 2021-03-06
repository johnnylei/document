## 反向代理与负载均衡
> 反向代理

如果我们需要访问某个服务器A，但是由于某种原因不能直接访问，而这个时候有服务器B可以访问服务器A,且我们可以访问服务器B,这时候我们可以通过服务器B来访问服务器A,这个叫做正向代理;

如果我们访问的资源在服务器A上，但是我们不知道服务器A,但是我们可以通过访问服务器B,获得我们想要的资源，这个就是反向代理;

正向代理和反向代理，主要区别在于
- 正向代理对于使用者来说，既要知道服务器还要知道代理服务器，对于代理服务器来说自身不需要服务器，使用者让他访问什么服务器他就访问什么服务器
- 反向代理对于使用者来说，只要知道代理服务器即可，但是对于代理服务器自身要知道服务器

> 负载均衡

当访问量和并发数特别大的时候，一台服务器难以支撑，这时候需要多台服务器协调。但是多台服务器同时作同一件事情，那么就要需要一个管理者来调度这个服务器集群来工作，而通过反向代理服务器正好可以充当这个管理者;

反向代理服务器代理一个服务器集群，当用户访问到代理服务器的时候，代理服务器便根据这个集群里面的机器运转的情况，去一个负载较轻的服务器上读取资源，这样就实现了负载均衡;

> nginx负载均衡
```
upstream myproject {
#   ip_hash; 　# 根据ip绑定集群里面指定的一台服务器，这样使得同一个用户每一次访问的都是同一个服务器
    server 123.56.193.192 weight=10; # 这里的server表示具体那个服务器，后面接ip地址和端口号，要在浏览器中可以访问的ip才能在这里生效，weight表示权重，默认值为１，weight越大表示访问的概率越大;
    server 114.215.140.70;
    server 139.129.229.82;
}

server {
    listen 80; 
    server_name fxproxy.local;
    location / { 
        proxy_pass http://myproject;　# 这里的myproject是上面的upstream里面的myproject,表示一个服务器集群的列表
    }   
}

```
