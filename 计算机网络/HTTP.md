# HTTP协议特点

* 支持客户/服务器模式
* 简单快速
* 灵活
* 无连接
* 无状态

# 请求/响应步骤

* 客户端连接Web服务器
* 发送HTTP请求
* 服务器接受请求并返回HTTP响应
*  释放连接TCP连接
* 客户端浏览器解析HTML内容

# 在浏览器输入URL后经历的流程

* DNS解析
  * 查询DNS缓存
    * 浏览器缓存
    * 系统缓存
    * 路由器缓存
    * IPS服务器缓存
    * 根域名服务器缓存
    * 顶级域名服务器缓存
* TCP连接
* 发送HTTP请求
* 服务器处理并返回HTTP报文
* 浏览器解析渲染页面
* 连接结束

# HTTP状态码

* 1xx	

  * 指示信息--表示请求已接收,继续处理
* 2xx

  * 成功--表示请求已被成功接收,理解,接收
  * 200 OK--正常的返回信息
* 3xx

  * 重定向--要完成请求必须进行更进一步的操作
  * 301--永久移动
  * 302--临时移动
  * 305--使用代理
* 4xx
  * 客户端错误--请求有语法错误或请求无法实现
  * 400 BadRequest--客户端请求有语法错误,不能被服务器所理解
  * 401 Unauthorized--请求未经授权,这个状态代码必须和WWW-Authenticate报头一起使用
  * 403 Forbidden--服务器收到请求,但是拒绝提供服务
  * 404 Not Found--请求资源不存在
* 5xx
  * 服务端错误--服务器未能实现合法的请求
  * 500 Internal Server Error-- 报务器发生不可预期的错误
  * 503 Server Unavailable--服务器当前不能处理客户端的请求,一段时间后可能恢复正常

# http如何保持连接 

* HTTP/1.0中默认使用的是短连接，每一次HTTP操作就建立一次连接，任务结束就中断
* HTTP/1.1默认使用长连接，并在响应头加入`Connection:keep-alive`
  * 当一个网页打开后，用于传输数据的TCP不会关闭，再次访问此服务器网页时会继续使用该连接
  * 实质是TCP协议的长连接和短连接

# 什么时候用长连接，短连接

* 长连接
  * 操作频繁，点对点的通讯，而且连接数不能太多
  * 如数据库的连接
* 知连接
  * Web的http服务，长连接对服务端会消耗一定资源，而web连接频繁使用短连接更节省资源

# GET请求和POST请求的区别

* HTTP报文层面
  * GET将请求信息放在URL(URl长度限制)
  * POST放在报文体中
* 数据库层面
  * GET符合幂等性和安全性
  * POST不符合
* 其他层面
  * GET可以被(CDN缓存,被存储
  * POST不可

# Cookie和Session的区别

* Session的实现方式
  * 使用Cookie来实现(JSESSIONID)
    * Session运行依赖session id，而session id存在cookie中，若浏览器禁用cookie，则session失效（可URL中传送session_id）
    * Session利用cookie进行信息处理,用户进行信息请求后,服务端就在浏览器上创建一个cookie,当这个session结束时,其实是Cookie过期了,它的`maxAge`属性一般为-1
  * 使用URL回写来实现
    * TOMCAT判断客户端浏览器是否支持Cookie的依据是请求中是否含有Cookie。尽管客户端可能会支持Cookie，但是由于第一次请求时不会携带任何Cookie（因为并无任何Cookie可以携带），URL地址重写后的地址中仍然会带有`jsessionid`。
    * 当第二次访问时服务器已经在浏览器中写入`Cookie`了，因此URL地址重写后的地址中就不会带有`jsessionid`了。
* 区别
  * Cookie数据存放在客户的浏览器上,Session数据放在服务器上
  * Session相对Cookie更安全
    * 若考虑减轻服务器负担,应当使用Cookie
  * Session能存储任意Java对象，Cookie只能存储String类型对象
* 相同点
  * 都是用来跟踪浏览器用户身份的会话方式
* Session什么时候失效

  * 服务器将长时间没有活动的Session从服务器内存中清除.Tomcat默认失效时间为20分钟
  * 调用Session的invalidate方法

# Session持久化

* 将Session对象从内存中转移到硬盘或数据库
  * 减少系统资源占用
  * 若Servlet容器关闭或重启，持久化的Session对象可重新加载，对于客户端还是使用同一session

# 如何防止cookie欺骗

* cookie加个加密算法
* 加个时间戳和IP戳,让cookie在同个IP下一定时间内失效
* 不管cookies里保存多少字段,还要增加一个验证字段(MAC码),使用上面所有字段的内容合算出来的摘要再用一种加密算法(如使用服务器的主密钥)加密.这样从cookie得到数据后,再判断一下MAC码可知cookies字段有没改过

# token

* 组成
  * 登陆时由服务端生成
  * UUID（用户唯一身份标识）+time（时间戳）+sign(签名)+[常用固定参数（可选）]
* 使用
  * 服务端生成后随http响应保存在客户端的cookies，或local storage中。随客户端请求发送至服务端，
  * 用于单点登陆身份验证，防止跨站点请求伪造等
* 有效期
  * 根据token中的时间戳跟当前时间对比计算，看过期与否，有效期默认7天
* 优缺点
  * 支持跨域访问，防止信息外泄，可以在多个服务间共享。不像session存储于服务器内存中，不影响服务器的性能，但需要额外的时间开销
  * 防止CSRF攻击
    * 登录受信任网站A，并在本地生成Cookie。
    * 在不登出A的情况下，访问危险网站B。

# Https与Http的区别

* HTTPS协议需要到ca申请证书,一般免费证书上较少,因而需要一定的费用

* HTTP是超文本传输协议,信息明文传输,HTTPS具有安全性的ssl加密协议

* Http端口80,HTTPS端口443

  

