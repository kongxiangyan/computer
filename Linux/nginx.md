Nginx配置文件主要分成四部分：main（全局设置）、server（主机设置）、upstream（上游服务器设置，主要为反向代理、负载均衡相关配置）和 location（URL匹配特定位置后的设置），每部分包含若干个指令。main部分设置的指令将影响其它所有部分的设置；server部分的指令主要用于指定虚拟主机域名、IP和端口；upstream的指令用于设置一系列的后端服务器，设置反向代理及后端服务器的负载均衡；location部分用于匹配网页位置（比如，根目录“/”,“/images”,等等）。他们之间的关系式：server继承main，location继承server；upstream既不会继承指令也不会被继承。它有自己的特殊指令，不需要在其他地方的应用。



nginx是一个高性能的web服务器，常用作反向代理服务器。nginx作为反向代理服务器，就是把http请求转发到另一个或者一些服务器上。 通过把本地一个url前缀映射到要跨域访问的web服务器上，就可以实现跨域访问。 对于浏览器来说，访问的就是同源服务器上的一个url。而nginx通过检测url前缀，把http请求转发到后面真实的物理服务器。并通过rewrite命令把前缀再去掉。这样真实的服务器就可以正确处理请求，并且并不知道这个请求是来自代理服务器的。 简单说，nginx服务器欺骗了浏览器，让它认为这是同源调用，从而解决了浏览器的跨域问题。又通过重写url，欺骗了真实的服务器，让它以为这个http请求是直接来自与用户浏览器的。 这样，为了解决跨域问题，只需要动一下nginx配置文件即可。



[使用nginx来为你在一台服务器部署多个Web Server使用nginx来为你在一台服务器部署多个Web Server](https://segmentfault.com/a/1190000014924401)

[Let's Encrypt 入门教程](https://bitmingw.com/2017/02/02/letsencrypt-tutorial/)



```bash
│                 npm update check failed                  │
│           Try running with sudo or get access            │
│           to the local update config store via           │
│ sudo chown -R $USER:$(id -gn $USER) /home/ubuntu/.config │
```



ruby

http://sinatrarb.com/documentation.html