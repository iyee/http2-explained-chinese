# http2的观念

所以http2到底定义了些什么呢？HTTPbis小组制定时的界限到底在哪儿呢？

其实界限是非常严格的，因此也给小组带来了很多限制

* [X] 它必须维持当前HTTP的范式。它依然是让客户端发送请求到服务器端的一个基于TCP的协议。

* [X] http://和https://的URLs不能被改变，也不能对其添加新的结构。使用这类URL的网站太多了，没法让他们全部改变。

* [X] HTTP1的服务器和客户端依然会存在很久，所以我们必须提供HTTP1到http2服务器的代理。

* [X] 我们也要让这种代理能够将http2的功能一对一的映射到HTTP 1.1的客户端。

* [X] 移出或者减少协议里面那些可选的部分。虽然这并不是一个真的要求，但是SPDY和Google的团队会非常喜欢这点。让协议里所有部分都成为强制也可能防止人们在实现的时候偷懒，从而避免将来的问题。

* [X] 不再使用小版本。服务器和客户端都必须决定自己是否兼容http2。如果将来该协议需要被扩充更变，那么新的协议将会是http3，而不是http 2.x。

**5.1. http2和现有的URI结构**

上文有提到现有的URI结构不能被改变，所以http2也必须使用当前的结构。因为HTTP 1.x正在使用当前的结构，所以我们必须需要升级当前协议到http2，或者让服务器用http2来彻底替代旧的协议。

HTTP 1.1本身就有定义“升级”的方案：提供一个头，让服务器在收到旧协议的时候，向客户端用新协议发送响应。这样做的代价是需要一个一次往返通信。

这个往返通信的代价是SPDY团队不能接受的，而且他们也只实现了基于TLS的SPDY。？？？。这个扩展被称作Next Protocol Negotiation(NPN)，服务器会向客户端列出他所支持的协议，从而让客户端从中选择一个合适的。

**5.2. http2 for https://**

让http2在TLS上正常工作花费了很多精力。SPDY只支持TLS，也强力的推动

只使用TLS的原因是这样可以更好的保护用户隐私，并且在早先的测试中发现新协议在TLS上通信成功率更高。

关于强制使用TLS的主题在邮件组和会议上引起了不小的争议

**5.3 http2在TLS上的协商**

Next Protocol Negotiation (NPN)是一个将SPDY部署到TLS服务器的协议。

**5.4 http2 for http://**

这到底是好是坏呢？请注意，如果你要参与HTTPbis关于这个话题的讨论，一定要小心。