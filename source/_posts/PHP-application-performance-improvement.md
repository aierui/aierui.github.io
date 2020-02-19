---
title: PHP 应用性能提升
art: a
id: 17062406
comments: false
tags:
  - PHP 
  - 性能
date: 2017-06-24 06:27:57
categories: PHP
---


如何保证服务高可用、高性能 是今天PHP全球开发者大全的主题，目前在业界已经有成熟的解决方案。在服务日常稳定，无太大波动时，很难考验服务的高可用，只有经历过爆发式的流量，暴露出问题才发现自己的“高可用”其实并不是那么回事。用户才是检验服务高可用的唯一依据。

<!-- more -->

在这里我将会从以下几个方面分析：

- [Nginx 与 Apache](#nginx-or-apache)

- [HTTP 服务器性能优化](#http)

- [内容分发网络 （CDN）](#cdn)

- [JavaScript/CSS 优化](#j-and-c)

- [全页缓存技术](#cache)

- [Varnish](#Varnish)

- [负载均衡](#LB)



<h2 id="nginx-or-apache">Nginx 与 Apache</h2>

### Apache 

Apache 处理请求的模式有三种：prefork 模式（线程创建进程）、worker模式（进程创建线程）、事件驱动模式（与worker模式相似，但会为连接创建专用线程，活动请求使用另外创建的线程）。Apache 在处理每一个请求都会由一个线程或者一个进程处理，，所以 Apache 在处理请求时，开销很大。当它在高并发场景下时，其性能底下的问题便会突显出来。

### Nginx

Nginx 提供异步、事件驱动、非阻塞请求处理，由于请求异步处理，Nginx 不必等待每个请求完成，避免锁住资源。Nginx 没有内置任何解释型语言，这也许是好事，因为如此一来Nginx便可以专注处理请求的接受与相应，而具体解析脚本语言的进程则在 Nginx 之外，通常我们认为 Nginx 要快于 Apache ，但是在一些场景下，例如静态资源下，Apache 也有自己的优势。

因此，在构建高性能服务器时，APache 并不是问题所在，PHP 才是真正的瓶颈。



### PHP-FPM 配置

```
	[shijinrong.web.cn]
	user = www
	group = www
	listen = 127.0.0.1:7890
	listen.allowed_clients = 127.0.0.1
	pm = dynamic
	pm.max_children = 50
	pm.start_servers = 10
	pm.min_spare_servers = 5
	pm.max_spare_servers = 20
	pm.max_requests = 1500
	slowlog = /data1/www/logs/$pool-slow_log
	request_slowlog_timeout = 2
	request_terminate_timeout = 30
	catch_workers_output = no
	security.limit_extensions = ""

	php_admin_value[include_path] = ".:/usr/local/lib/php" 
```

配置：[官方解释](http://php.net/manual/zh/install.fpm.configuration.php)


> 如何最优化呢？
 
就需要和自己的服务状况、机器配置性能等紧密结合



<h2 id="http">HTTP 服务器性能优化</h2>

### HTTP Server 优化

#### 缓存静态文件

通常将静态资源（img、css、js、font）变更不频繁 添加特殊的响应头

> Apahce 在 .htaccess 文件中添加
```
<FilesMatch "\.(ico|jpg|jpeg|png|gif|css|js|woff)$">
	Header set Cache-Control "max-age=604800"
</FilesMatch>
```

> Nginx 在主机的host配置 conf 
```
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|css|js|woff)$
{
 	expires 7d;
}
```

这里 大家可能会有疑问？要是我这7天内修改了静态文件怎么办呢？

好了，这个问题就需要引入版本号，在静态文件之后 也增加一个参数 作为区分


### HTTP 持久链接

> HTTP keep-alive 模式的优点

- CPU 和内存的负载会减轻，因为同一时刻打开的 TCP 链接数变少了，后续请求和响应无需打开新链接，可以继续基于这个 TCP 链接发送上下行数据。

- 当 TCP 链接建立后，请求的等待时间将会减少，TCP 建立链接时的三次握手发生在用户与Server之间，当🤝成功，一条TCP链接就被建立起来了，同一个域的请求可以并行下载资源(chrome 最大为6)、

- 网络阻塞情况减轻，因为同一时刻是会有少数的链接保持着。



Apache 开启 Keep-alive 方式有两种，分别是通过修改 .htaccess 文件或 Apache 配置文件

Nginx 的 Keep-alive 是由 http_core 模块支持的，默认情况下是开启的。


除了，缓存静态资源、开启长链接。我们还可以开启 GZIP 压缩功能

### PHP独立部署服务

Apache 是以 mod_php 模块的方式加载 PHP 的。这样，PHP 与 Apache 耦合很紧，所有的请求都会由 Apache 模块处理，非常消耗机器的硬件资源，我们可以让 PHP-FPM 与 Apache 结合，独立部署，通过 FastCGI 协议及相互传递数据。这样子 Apache 只需要关注 HTTP 请求链接即可，PHP 进程则有 PHP-FPM创建和维护。


Nginx 则有些不同，Nginx 不提供内建 PHP 模块的方法，所以 Nginx 与 PHP 本身就是相互独立的。

最后就是关闭没有服务、没用的模块。


<h2 id="cdn">内容分发网络 （CDN）</h2>

### CDN网络特征如下

- 内容是静态的，不频繁更爱，因为 CDN 会将它们缓存在内存中。

- CDN 服务器位于不同的位置。当浏览器请求到达 CDN 时，CDN从请求位置可用的最近位置发送请求内容。

- 每个浏览器都具有向域发送同时请求的限制



自己在项目中使用 CDN，是这样子的 “卧槽，快了好多”；使用完后 “5555，还能不能更快点啊”

免费的CDN资源库

- [](http://www.bootcdn.cn/)


<h2 id="j-and-c">JavaScript/CSS/Img 优化</h2>

静态资源围绕 **合并** **压缩** 

合并 可以减少资源请求数量

压缩 可以减少资源下载的时间

常用的资源压缩在线资源

- [tinypng图片压缩](https://tinypng.com/)

- [minifier](http://www.minifier.org/)


当然，最优的方案就是使用前端构建工具(grunt、webpack) 打包

<h2 id="cache">全页缓存技术</h2>

使用这个条件是 网页不经常做变更，一般可以用在企业产品官方等。

大多数的平台通过内置支持货通过插件和幂快实现全页缓存。在这种情况下，插件或模块为每个请求处理页面的动态区域。

<h2 id="Varnish">Varnish</h2>

** Varnish-Web应用程序加速器 **

官方文档 [Varnish Documentation](http://www.varnish-cache.org/docs/index.html)


<h2 id="LB">负载均衡</h2>

** HAProxy **

官方文档 [http://www.haproxy.org/](http://www.haproxy.org/)






