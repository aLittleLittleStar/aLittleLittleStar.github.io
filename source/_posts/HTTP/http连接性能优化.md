---
title: http连接性能优化
date: 2018-10-14 10:23:06
tags: http
categories: 前端
---
<!-- toc -->


### HTTP和TCP/IP的关系

HTTP—>(TSL/SSL)—>TCP—>IP 
HTTP处于应用层、TCP处于传输层、IP处于网络层
1. HTTP将所需要传输的数据以流的形式传递给TCP程序
2. TCP解析数据中的IP地址和端口号，将数据流分割成数据段，并添加上TCP段首部，如TCP握手（ACK、SYNC等），源端口，目的端口、TCP校验和等
3. TCP程序将包装好的TCP数据段叫给IP程序，IP程序在此基础上封装进去IP分组首部，如源IP地址、目的IP地址，数据报总长度、分组ID、首部长度、首部校验和等等
4. 最后交给数据链路层去发送这个IP分组数据段
<!-- more -->
### TCP性能的考虑
__HTTP紧挨着TCP，所以TCP的链接性能考虑直接影响的HTTP事务的性能。__

#### HTTP事务时延

一次HTTP请求可分为 __DNS查询__、__连接__、__请求__、__事务处理__、__响应__、__关闭连接__。每一步都会产生时延。其中，相对于__连接__、__请求__所消耗的时间，***事务处理***的时间是很短的。

对HTTP程序员产生影响的时延 
+ TCP握手建立链接 
+ TCP慢启动拥塞控制 
+ 数据聚集的Nagle算法 
+ 用于捎带确认的TCP延迟确认算法 
+ TIME_WAIT 时延和端口耗尽

#### HTTP连接处理
##### Connection首部真正用途
HTTP允许客户端和源服务器之间存在多个代理服务器或高速缓存服务器，进行HTTP连接通信时，可以将HTTP首部逐跳的经过这些设备。这个时候，怎么在相邻的HTTP应用程序之间的连接应用一些特殊的选项呢？— Connection首部，可以承载3种不同类型的标签，这些标签不会传播到其它连接中去。 
a、HTTP首部字段名，列出了只与此连接有关的选项 
b、任意标签，用于描述此连接的非标准选项 
c、值close，说明操作完成之后需关闭这条持久连接 
由于添加Connection首部的其它首部字段，不能随着报文转发出去。因此将逐跳首部放入Connection首部，就可以达到对首部的保护。 
例：
``` js
HTTP/1.1 200 OK
Cache-control: max-age=3600
Connection: meter,close,bill-my-credit-card
Meter: max-uses=3,max-refuses=6,dont-report
```
实例说明：不应该转发Meter首部，要应用假想的bill-my-credit-card选项，且本次事务后应关闭持久连接。

##### 串行事务处理延迟 
如果只对HTTP事务进行简单管理，TCP的性能时延可能会叠加起来，包括多次的建立连接和断开连接。

#### 提高HTTP连接性能的四个方法： 
#####  并行连接 
通过多条TCP连接发起并发的HTTP请求 
并行连接从理论上回提高页面的加载速度，因为多个请求同时发出，时延可以重叠起来。 
但并行连接并不是一点更快，原因可能是：客户端带宽限制、消耗更多的内存和计算资源。 
现代浏览器确实使用并行连接，但会限制连接数在一个较小的值（通常是4），并且服务器可以关闭来自特定客户端的超量连接。

#####  持久连接 
重用TCP连接，以消除连接及关闭的时延 
重用连接：HTTP/1.1(HTTP/1.0增强版)允许HTTP设备在事务处理结束后将TCP连接保持在打开状态，以便为未来的HTTP请求重用现存的连接。

###### 持久连接+并行连接 
持久的连接的管理很重要，不小心会累积出大量的空闲连接

######  HTTP/1.0 + Keep-alive连接 
Connection： Keep-alive属性出现在1996年HTTP/1.0版本中，当初也是被当做实验型持久连接。

可以用通用首部Keep-Alive属性指定由逗号分隔的选项来调节keep-alive的行为。例：
``` js
Connection：Keep-alive
Keep-Alive: max=5,timeout=120
```
说明：服务器还会为另外5个事务保持连接的打开状态，或者将打开状态保持到连接空闲后2分钟。

Connection属于逐跳首部，只适用于单条传输链路。

现在HTTP/1.1不再需要此属性，默认开启持久连接的。

###### HTTP/1.1 持久连接 
HTTP/1.1逐渐停止了对keep-alive连接的支持，用一种名为持久连接(persistentconnection)的改进型设计取代了它。

必须显示指定Connection: close才会指定TCP连接在响应后立即关闭。当客户端发送了Connection: close请求首部之后，客户端就无法在那条连接上发送更多请求了。 
只有当连接上所有的报文都有正确的、自定义报文长度时，连接才能持久保持。

##### 管道化连接 
通过共享的TCP连接发起并发的HTTP请求

HTTP/1.1 允许在持久连接上可选的使用请求管道。在响应到达之前，可以将多条请求放入队列，降低网络回环时间。

注：HTTP客户端不应该用管道化的方式发送会产生副作用的请求（比如POST），因为出错时，无法安全的重试POST这样的非幂请求。

##### 复用的连接 
交替传送请求和相应报文（实验阶段）




### HTTP1.1 的特点
#### 持久连接
每个TCP连接开始都有三次握手，要经历一次客户端与服务器间完整的往返，而开启了持久连接就不需要每次都要握手
![开启Keep-Alive](/assets/images/keep-alive.png)
在连接中有这个属性的就是打开了持久化连接。下图展示了通过持久 TCP 连接取得 HTML 和 CSS 文件：
![开启Keep-Alive](/assets/images/keep-alive1.jpg)

### HTTP2.0 的特点

### HTTP 长连接与短连接
__HTTP  是无状态的__
也就是说，浏览器和服务器每进行一次HTTP操作，就建立一次连接，但任务结束就中断连接。如果客户端浏览器访问的某个HTML或其他类型的Web页中包含有其他的Web资源，如JavaScript文件、图像文件、CSS文件等；当浏览器每遇到这样一个Web资源，就会建立一个HTTP会话.
http1.0中默认是关闭的，需要在http头加入"Connection: Keep-Alive"，才能启用Keep-Alive
http 1.1中默认启用Keep-Alive，如果加入"Connection: close"才关闭。
目前大部分浏览器都是用http1.1协议，也就是说默认都会发起Keep-Alive的连接请求了，所以是否能完成一个完整的Keep- Alive连接就看服务器设置情况。
下图是普通模式和长连接模式的请求对比： 
![普通模式和长连接模式的请求对比](/assets/images/keep-alive2.png)
#### 开启Keep-Alive的优缺点
优点： Keep-Alive模式更加高效，因为避免了连接建立和释放的开销
缺点： 长时间的Tcp连接容易导致资源无效占用，浪费系统资源
#### 当保持长连接时，如何判断一次请求已经完成？ 
当保持长连接时，如何判断一次请求已经完成？ 
##### Content-Length 
Content-Length表示实体内容的长度。浏览器通过这个字段来判断当前请求的数据是否已经全部接收。 
所以，当浏览器请求的是一个静态资源时，即服务器能明确知道返回内容的长度时，可以设置Content-Length来控制请求的结束。但当服务器并不知道请求结果的长度时，如一个动态的页面或者数据，Content-Length就无法解决上面的问题，这个时候就需要用到Transfer-Encoding字段。

##### Transfer-Encoding 
Transfer-Encoding是指__传输编码__，在上面的问题中，当服务端无法知道实体内容的长度时，就可以通过指定Transfer-Encoding: chunked来告知浏览器当前的编码是将数据分成一块一块传递的。当然, 还可以指定Transfer-Encoding: gzip, chunked表明实体内容不仅是gzip压缩的，还是分块传递的。最后，当浏览器接收到一个长度为0的chunked时， 知道当前请求内容已全部接收。

#### Keep-Alive timeout： 
Httpd守护进程，一般都提供了keep-alive timeout时间设置参数。比如nginx的keepalive_timeout，和Apache的KeepAliveTimeout。这个keepalive_timout时间值意味着：一个http产生的tcp连接在传送完最后一个响应后，还需要hold住keepalive_timeout秒后，才开始关闭这个连接。 
当httpd守护进程发送完一个响应后，理应马上主动关闭相应的tcp连接，设置 keepalive_timeout后，httpd守护进程会想说：”再等等吧，看看浏览器还有没有请求过来”，这一等，便是keepalive_timeout时间。如果守护进程在这个等待的时间里，一直没有收到浏览器发过来http请求，则关闭这个http连接。

#### Tcp的Keepalive： 
连接建立之后，如果客户端一直不发送数据，或者隔很长时间才发送一次数据，当连接很久没有数据报文传输时如何去确定对方还在线，到底是掉线了还是确实没有数据传输，连接还需不需要保持，这种情况在TCP协议设计中是需要考虑到的。 
TCP协议通过一种巧妙的方式去解决这个问题，当超过一段时间之后，TCP自动发送一个数据为空的报文（侦测包）给对方，如果对方回应了这个报文，说明对方还在线，连接可以继续保持，如果对方没有报文返回，并且重试了多次之后则认为链接丢失，没有必要保持连接。

tcp keep-alive是TCP的一种检测TCP连接状况的保鲜机制。tcp keep-alive保鲜定时器，支持三个系统内核配置参数： 
net.ipv4.tcp_keepalive_intvl = 15 
net.ipv4.tcp_keepalive_probes = 5 
net.ipv4.tcp_keepalive_time = 1800 
keepalive是TCP保鲜定时器，当网络两端建立了TCP连接之后，闲置（双方没有任何数据流发送往来）了tcp_keepalive_time后，服务器就会尝试向客户端发送侦测包，来判断TCP连接状况(有可能客户端崩溃、强制关闭了应用、主机不可达等等)。如果没有收到对方的回答(ack包)，则会在 tcp_keepalive_intvl后再次尝试发送侦测包，直到收到对方的ack,如果一直没有收到对方的ack,一共会尝试 tcp_keepalive_probes次，每次的间隔时间在这里分别是15s, 30s, 45s, 60s, 75s。如果尝试tcp_keepalive_probes,依然没有收到对方的ack包，则会丢弃该TCP连接。TCP连接默认闲置时间是2小时，一般设置为30分钟足够了。










参考文章： [浅谈Http长连接和Keep-Alive以及Tcp的Keepalive](https://blog.csdn.net/weixin_37672169/article/details/80283935)、[http性能优化](https://blog.csdn.net/joye123/article/details/51931375?utm_source=itdadao&utm_medium=referral)