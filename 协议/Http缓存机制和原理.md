### Http缓存机制和原理

#### 1.Http报文

通俗点说Http报文就是客户端和服务器在使用Http通信过程中的数据块。

客户端向服务器请求数据会发送请求(request)报文，服务器向客户端返回数据会发送响应(response)报文。

报文数据主要包含两部分：
a.包含各种属性(附加信息)的头部(header)
包括cookie,缓存信息等，**注：与缓存相关的规则信息均包含在header中**
b.包含数据的主题部分(body)
包含Http真正想要传输的数据

#### 2.缓存规则

为了方便理解，我们可以认为客户端存在有一个缓存数据库(实际不一定是数据库啊，也有可能是文件)。

不管什么样的缓存规则，一个Url在第一次访问服务器的时候的流程都是相同的：

![Cache_1](https://upload-images.jianshu.io/upload_images/5231076-3165cc6b85c5bde5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

先从缓存数据库中获取数据，发现没有，再请求网络从服务器获取数据和缓存规则，然后客户端将数据和缓存规则存入缓存数据库。

问题：url在第一次访问服务器的时候不一定会先从缓存数据库中读取数据，是否有可能直接就去访问服务器，有待验证？

HTTP缓存有多种规则，我们可以大概分为两种：强制缓存和对比缓存(协商缓存)

大前提：本地已有缓存数据
仅支持强制缓存，请求数据的流程如下：

![Cache_2](https://upload-images.jianshu.io/upload_images/5231076-15bea2b988a6e94e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在强制缓存规则下，客户端先从缓存数据库中读取数据，如果发现有缓存数据且未失效，则返回数据并使用；当发现缓存数据库中的数据已失效，客户端会请求服务器，服务器返回数据和缓存规则，客户端再把数据和缓存规则存入缓存数据库。

仅支持对比缓存，请求数据的流程如下：

![Cache_3](https://upload-images.jianshu.io/upload_images/5231076-2051c5cf0004cffb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对比缓存每次都会请求网络，大概是这样的：客户端先去缓存数据库中获取缓存数据的标识，然后客户端请求服务器验证缓存标识对应的缓存数据是否失效，如果未失效，客户端直接读取缓存中的数据，如果失效了，服务器会返回最新的数据和缓存标识，客户端再把数据和缓存标识存入缓存数据库中。

**小结**

- 从上面规则可以看出强制缓存如果生效了，客户端跟服务器是没有交互的，但是在对比缓存中，不论缓存是否生效，都会向服务器发送请求；

- 强制缓存和对比缓存是可以共存的，优先级是强制缓存>对比缓存，即如果强制缓存生效，就不会执行对比缓存。

#### 3.强制缓存

从前文可知，强制缓存中如果数据还有效，客户端会直接使用缓存数据，那么客户端是根据什么判断缓存数据是否有效的呢？

在强制缓存中，每次客户端只要向服务器请求数据，服务器就会把数据和缓存规则返回来，缓存规则的信息就在响应header中，主要有两个字段来标明失效规则：Expires/Cache-Control。

**Expires**

Expires是HTTP 1.0中的东西，现在HTTP协议基本都是1.1，所以这个字段基本废弃不用了，但是还是要解释一下它的意思：Expires的值是服务器返回的数据到期时间，客户端下次请求的时候会去比较当前时间与Expires的值，如果小于Expires的值，直接读取缓存中的数据，如果大于Expires的值，说明数据过期了，则需要请求服务器获取最新数据。

**Cache-Control**

Cache-Control字段的值可以分为以下几类：

可缓存性：
private:表明客户端可以缓存响应，但是响应不能作为共享缓存(即代理服务器不能缓存该响应)；

public:表明响应可以被任何对象缓存(包括客户端、代理服务器等)；

no-cache:强制缓存了该响应的客户端，在使用已缓存的数据前，发送带验证器的请求到服务器验证该请求的数据是否已失效(对比缓存)；

no-store:所有内容都不会被缓存，强制缓存和对比缓存都不会被触发；

only-if-cached:表明如果缓存存在，只使用缓存，无论原始服务器数据是否改变；

到期：
max-age=xxx:设置缓存存储的最大周期，超过这个时间缓存就被认为是过期的，这是一个相对时间，单位一般为秒，强制缓存一般会设置该字段的值；

s-maxage=xxx:客户端缓存一般忽略该字段，仅适用于共享缓存(比如代理服务器)，它的优先级要高于max-age；

max-stale=xxx:表明客户端愿意接受一个已经过期的资源(**不是特别理解，黑人问号脸**)；

min-fresh=xxx:表示客户端希望在指定的时间内获取最新的响应；

#### 4.对比缓存

客户端在第一次请求数据时，服务器会把缓存标识和数据一起返回给客户端，客户端将二者备份到缓存数据库中，再次请求时客户端将缓存标识发送给服务器，服务器根据缓存标识判断当前请求的资源有没有改变，如果没有变化，返回304状态码，告知客户端比较成功，数据没有变化，可以使用缓存，如果数据有变化，服务器会返回最新的数据和缓存标识，客户端再把最新的数据和缓存标识备份到缓存数据库。

![Cache_4](https://upload-images.jianshu.io/upload_images/5231076-0c2f21d4089cc85e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上面截图我们可以看到对比缓存生效时，状态码为304，并且报文的大小以及响应时间大大减少。

在对比缓存中有两组资源标识字段非常重要：

**Last-Modified/If-Modified-Since**

Last-Modified:该字段是服务器在响应请求时告诉客户端资源的最后修改时间。

![Cache_5](https://upload-images.jianshu.io/upload_images/5231076-e3f7408741749275.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

If-Modified-Since:当客户端有缓存了，再次请求时通过此字段通知服务器上次请求时服务器返回的资源最后修改时间。服务器收到请求后发现有头部字段If-Modified-Since，则会把该字段的值与被请求资源的最后修改时间做比较，如果资源的最后修改时间大于该值，说明资源被改动过，服务器响应整片资源内容，返回状态码200，如果资源的最后修改时间小于等于该值，说明资源没有新修改，返回状态栏304，告知客户端可以使用缓存。

![Cache_6](https://upload-images.jianshu.io/upload_images/5231076-106f34c7b7adeab0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Etag/If-None-Match**

Etag：服务器响应请求时，返回的资源唯一标识(生成规则由服务器决定)。

![Cache_7](https://upload-images.jianshu.io/upload_images/5231076-840816182e8b5adb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

If-None-Match：当客户端存在缓存再次请求服务器时，通过此字段告知服务器客户端缓存的数据唯一标识，服务器收到请求后发现有头部字段If-None-Match，会把该字段的值与被请求的资源唯一标识做比较，如果不同，说明资源被修改过，响应整片资源内容，返回状态码200，如果相同，说明资源没有被修改过，返回状态码304，告知客户端可以使用缓存。

![Cache_8](https://upload-images.jianshu.io/upload_images/5231076-7b9d496fe0ca2cbd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**总结**
- 对于强制缓存，如果下次请求的时间在max-age范围内，则直接使用缓存，超出这个时间范围，执行对比缓存策略；
- 对于对比缓存策略，客户端会把Etag或者Last-Modified通过请求发送给服务器，由服务器来校验缓存是否可用，返回状态码304，表示缓存可用。

![Cache_9](https://upload-images.jianshu.io/upload_images/5231076-a3bfe2c19ace3206.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 参考
[彻底弄懂HTTP缓存机制及原理](http://www.cnblogs.com/chenqf/p/6386163.html)