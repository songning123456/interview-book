#### SPDY协议？
Google在2010年发布了SPDY(取自SPeeDY，发音同 speedy)，其开发目标旨在解决HTTP的性能瓶颈，缩短Web页面的加载时间。


在Facebook和Twitter等SNS网站上，几乎能够实时观察到海量用户公开发布的内容，这也是一种乐趣。当几百、几千万的用户发布内容时，Web网站为了保持这些新增的内容，在很短的时候内就会发生大量的内容更新。为了尽可能实时地显示这些更新内容，服务器上一有内容更新，就需要直接把那些内容反馈到客户端的界面上。虽然看起来挺简单的，但HTTP却无法妥善地处理好这项任务。使用HTTP协议探知服务器上是否有内容更新，就必须频繁地从客户端到服务器端进行确认。如果服务器上没有内容更新，那么就会产生徒劳的通信。


```
HTTP瓶颈：
1. 一条连接上只可发送一个请求。
2. 请求只能从客户端开始。客户端不可以接收除响应以外的指令。
3. 请求/响应首部未经压缩就发送。首部信息越多延迟越大。
4. 发送冗长的首部。每次互相发送相同的首部造成的浪费较多。
5. 可任意选择数据压缩格式。非强制性压缩发送。
```


SPDY没有完全改写HTTP协议，而是在TCP/IP的应用层与传输层之间通过新加 会话层的形式运作。同时，考虑到安全性问题，SPDY在规定通信中使用SSL。SPDY以会话层的形式加入，控制对数据的流动，但还是采用HTTP建立通信连接。因此，可照常使用HTTP的GET和POST等方法、Cookie以及HTTP报文等。


![SPDY](/images/Web/SPDY.png)


使用SPDY后，HTTP协议额外获得的功能。


| 功能 | 解释 | 
| :----- | :----- | 
|<div style='width: 120px'>多路复用</div>|通过单一的TCP连接，可以无限制地处理多个HTTP请求。所有请求的处理都在一条TCP连接上完成，因此TCP的处理效率得到提高。|
|<div style='width: 120px'>赋予请求优先级</div>|SPDY不仅可以无限制地并发处理请求，还可以给请求逐个分配优先级顺序。这样主要是为了在发送多个请求时，解决因带宽低而导致响应变慢的问题。|
|<div style='width: 120px'>压缩HTTP首部</div>|压缩HTTP请求和响应的首部。这样一来，通信产生的数据包数量和发送的字节数就更少了。|
|<div style='width: 120px'>推送功能</div>|支持服务器主动向客户端推送数据的功能。这样，服务器可直接发送数据，而不必等待客户端的请求。|
|<div style='width: 120px'>服务器提示功能</div>|服务器可以主动提示客户端请求所需要的资源。由于在客户端发现资源之前就可以获知资源的存在，因此在资源已缓存等情况下，可以避免发送不必要的请求。|


使用SPDY后，Web的内容端不必做什么特别改动，而Web浏览器及Web服务器都要为对应SPDY做出一定程度的改动。因为SPDY基本上只有将单个域名(IP地址)的通信多路复用，所以当一个Web网站上使用多个域名下的资源，改善效果就会受到限制。


#### WebSocket是什么？
WebSocket协议在2008年诞生，2011年成为国际标准。所有浏览器都已经支持了。WebSocket同样是HTML 5规范的组成部分之一，现标准版本为RFC 6455。WebSocket相较于上述几种连接方式，实现原理较为复杂，用一句话概括就是：


客户端向WebSocket服务器通知(notify)一个带有所有接收者ID(recipients IDs)的事件(event)，服务器接收后立即通知所有活跃的(active)客户端，只有ID在接收者ID序列中的客户端才会处理这个事件。由于WebSocket本身是基于TCP协议的，所以在服务器端我们可以采用构建TCP Socket服务器的方式来构建WebSocket服务器。这个WebSocket是一种全新的协议。它将TCP的Socket(套接字)应用在了web page上，从而使通信双方建立起一个保持在活动状态连接通道，并且属于全双工(双方同时进行双向通信)。


其实是这样的，WebSocket协议是借用HTTP协议的101 switch protocol来达到协议转换的，从HTTP协议切换成WebSocket通信协议。


#### WebSocket有什么特点？
1. 服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。
2. 建立在TCP协议之上，服务器端的实现比较容易。
3. 与HTTP协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用HTTP协议，因此握手时不容易屏蔽，能通过各种HTTP代理服务器。
4. 数据格式比较轻量，性能开销小，通信高效。
5. 可以发送文本，也可以发送二进制数据。
6. 没有同源限制，客户端可以与任意服务器通信。
7. 协议标识符是ws(如果加密，则为wss)，服务器网址就是URL。


#### HTTP/2.0的新特性？
HTTP2.0大幅度的提高了Web性能，在HTTP1.1完全语意兼容的基础上，进一步减少了网络的延迟。实现低延迟高吞吐量。对于前端开发者而言，减少了优化工作。主要有以下几个新特性：


1. 二进制分帧
2. 首部压缩
3. 流量控制
4. 多路复用
5. 请求优先级
6. 服务器推送


HTTP/2相比于HTTP/1，可以说是大幅度提高了网页的性能。在HTTP/1中，为了性能考虑，我们会引入雪碧图、将小图内联、使用多个域名等等的方式。这一切都是因为浏览器限制了同一个域名下的请求数量(Chrome下一般是限制六个连接)，当页面中需要请求很多资源的时候，队头阻塞(Head of line blocking)会导致在达到最大请求数量时，剩余的资源需要等待其他资源请求完成后才能发起请求。在HTTP/2中引入了多路复用的技术，这个技术可以只通过一个TCP连接就可以传输所有的请求数据。多路复用很好的解决了浏览器限制同一个域名下的请求数量的问题，同时也间接更容易实现全速传输，毕竟新开一个TCP连接都需要慢慢提升传输速度。大家可以通过该链接感受下HTTP/2比HTTP/1到底快了多少。


在HTTP/2中，因为可以复用同一个TCP连接，你会发现发送请求是长这样的：


| 名称 | 解释 | 
| :----- | :----- | 
|<div style='width: 90px'>二进制传输</div>|HTTP/2中所有加强性能的核心点在于此。在之前的HTTP版本中，我们是通过文本的方式传输数据。在HTTP/2中引入了新的编码机制，所有传输的数据都会被分割，并采用二进制格式编码。|
|<div style='width: 90px'>多路复用</div>|在HTTP/2中，有两个非常重要的概念，分别是帧(frame)和流(stream)。帧代表着最小的数据单位，每个帧会标识出该帧属于哪个流，流也就是多个帧组成的数据流。多路复用，就是在一个TCP连接中可以存在多条流。换句话说，也就是可以发送多个请求，对端可以通过帧中的标识知道属于哪个请求。通过这个技术，可以避免HTTP旧版本中的队头阻塞问题，极大的提高传输性能。|
|<div style='width: 90px'>Header压缩</div>|在HTTP/1中，我们使用文本的形式传输header，在header携带cookie的情况下，可能每次都需要重复传输几百到几千的字节。在HTTP/2中，使用了HPACK压缩格式对传输的header进行编码，减少了header的大小。并在两端维护了索引表，用于记录出现过的header ，后面在传输过程中就可以传输已经记录过的header的键名，对端收到数据后就可以通过键名找到对应的值。|
|<div style='width: 90px'>服务端Push</div>|在HTTP/2中，服务端可以在客户端某个请求后，主动推送其他资源。可以想象以下情况，某些资源客户端是一定会请求的，这时就可以采取服务端push的技术，提前给客户端推送必要的资源，这样就可以相对减少一点延迟时间。当然在浏览器兼容的情况下你也可以使用prefetch。|