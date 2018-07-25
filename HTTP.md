### HTTP协议状态码

##### 零、 1XX信息响应

100 Continue
这个临时响应表明，迄今为止的所有内容都是可行的，客户端应该继续请求，如果已经完成，则忽略它。
101 Switching Protocol
该代码是响应客户端的Upgrade：request标头发送的，并且指示服务器也正在切换的协议。
102 Processing (WebDAV)
此代码表示服务器已收到并正在处理该请求，但没有响应可用。

##### 一 、 2XX 类型状态码——成功状态码

1、200  ok  表示从客户端发送的请求被服务端正确的处理并且已经发回了请求。

2、204  No Content 请求已经成功了，但是却没有返回任何结果（实体）。一般在只需要从客户端往服务器发信息而服务器不需要对客户端发送新信息内容。

3、206  Partial Content 该状态码表示客户端进行了范围请求，而服务器成功执行了这部分的GET请求。



##### 二、3XX状态码——重定向状态码

  1、301 状态码 **永久重定向** Moved Permanently  表示你请求的页面资源现在已经转移位置了，你需要到新的地方去需找该页面。这个即重定向，服务器的response首部里会有location字段值来提示。

   2、302 状态码 **临时重定向**(Found)和301差不过。表示你请求的页面资源现在已经转移位置了，你要到新的地方去寻找。但是新的地方也不是固定的，说不定过几天还要换。不提示用户保存书签，提示用户跳转。

   3、303 状态码。See other 与302相似，但是明确表示客户端应该采用GET方法获取资源。

   4、304 Not Modified 表示资源已经找到了，但是和上次相比没有更新。浏览器读取缓存。

   5、307 Temporary Redirect 临时重定向。与302一样，但是严格执行浏览器标准，不将POST变为GET。

> 当301，302，303状态码返回时，几乎所有的浏览器都会把POST改成GET，并删去报文内的主体，之后请求自动再次发送。尽管301，302的标准是禁止这样做的。

##### 三：4XX——客户端错误状态码

   1、400 Bad Request 报文中有语法错误。

   2、401 Unauthorized 需要通过HTTP认证（BASIC 或者DIGEST）.

   3、403 Forbidden 未获得访问授权，被服务器拒绝。

   4、404 服务器上无此资源，一般情况为路径错误。

##### 四：5XX ——服务端错误

   1、500 Internal Server Error   服务端错误，有可能是WEB应用存在错误。

​    2、502 Bad Gateway 作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应

​    3、503 Serveice Unavailable   服务器超负荷运行或停机维护，无法处理请求。



### HTTP：方法

#### GET：获取资源

​	GET提交，请求的数据会附在URL之后（就是把数据放置在HTTP协议头中），以?分割URL和传输数据，多个参数用&连接；例 如：login.action?name=hyddd&password=idontknow&verify=%E4%BD%A0%E5%A5%BD。如果数据是英文字母/数字，原样发送，如果是空格，转换为+，如果是中文/其他字符，则直接把字符串用BASE64加密，得出如： %E4%BD%A0%E5%A5%BD，其中％XX中的XX为该符号以16进制表示的ASCII。

#### POST: 传输信息

#### PUT：传输文件

#### HEAD：获取报文首部

#### DELETE：删除指定的资源

#### OPTIONS：询问支持的方法



### HTTP 报文通用首部字段

#### Cache-Control：

* no-cache：
  * 客户端：请求不走缓存服务器，直接转发给源服务器。（不接收缓存过的响应。
  * 服务端：缓存服务器不能对资源进行缓存。

* no-store 两端都不进行缓存。
* max-age=Number 优先级高于Expires，代表资源保存为缓存的最长时间
* min-fresh=Number 要求缓存服务器返回未超过指定时间的缓存资源。
* max-stale=Number 即使过期，但在max-stale指定时间内，客户端仍旧接收。
* only-if-cached 只从缓存服务器里取资源，不重新加载响应，也不确认资源有效性，若无响应返回504状态码。
* must-revalidate ：**代理**会向原服务器再次验证缓存是否有效。若无，则返回504
* Proxy-revalidate: **缓存服务器**在接收到带有该指令的客户端请求时，必须验证缓存的有效性。
* no-transform 缓存不能改变实体主体的媒体类型，防止缓存或代理压缩图片等操作。

#### Connection

控制不再转发给代理的首部字段，管理持久连接。

connection： 不再转发的字段 or close/keep-alive

#### Date

HTTP报文创建的日期和时间。

```
Date： Tue, 03 jul 2017 05:05:55 GMT
```

#### Pragma

只用于客户端发送的请求，要求所有中间服务器不返回缓存的资源。

与Cache-Control:no-cache共同作用，其作用向后兼容而定义的（HTTP/1.0)，且唯一（Pragma：no-cache）。

#### Trailer

说明在报文主体后记录了哪些首部字段。

```
Trailer: Expires
···报文主体
Expires： ···
```



#### Transfer-Encoding

规定传输报文主体时采用的编码方式。

Transfer-Encoding： chunked

#### Upgrade

用于检测 HTTP协议或者其他协议是否可以使用更高的版本通信，参数可用来指定协议。

需要搭配Connection：Upgrade使用。

```

Upgrade： TLS/1.0

Connection： Upgrade

```

#### Via

追踪报文的传输路径，还可避免请求回环的发生。

#### Warning

告知用户与缓存相关问题的警告。

```
Warning: [警告码][警告的主机：端口号]“[警告内容]”([日期时间])
```



### 请求首部字段

#### Accept

通知服务器，用户代理能处理的媒体类型及其优先级。给媒体类型增加优先级，用分号(;)进行分隔，q=来表示权重值。权重默认为1，最大为1。

```
Accept： text/html,application/xhtml+xml;q=0.9,*/*;q=0.8
```

#### Accept-Charset

通知服务端，用户代理支持的字符集及其优先级。

```
Accept-Charset: iso-8859-5, unicode-1-1;q=0.9
```

#### Accept-Encoding

通知服务端，用户代理支持的内容编码格式，及其优先级。

```
Accept-Encoding： gzip, deflate;q=0.9
```

#### Accept-Language

用户代理能处理的语言集及其优先级

```
Accept-Language： zh-cn;1.0
```

#### Authorization

告知服务器，用户代理的认证信息。

```
Authorization: Basic fkajdlkjAAKLDasd···
```

#### Expect

告知服务器，期待出现某种待定行为。

```
Expect: 100-continue	//只定义了这个
```

#### From

告知服务器用户代理的邮件地址。

```
From:info@hacker.cn
```

#### HOST

告知服务器，请求的资源所处的互联网主机名和端口号。

当一个IP地址部署着多个域名，服务器就需要根据Host来明确发出请求的主机名。

#### If-xxxx

条件请求。服务器接收到附带条件的请求，只有判定指定条件为真时，才会执行请求。

```
If-Match：“123456” //与资源的实体标识Etag匹配
If-None-Match：*//与资源的实体标识Etag不匹配时
If-Modified-Since：Tue, 03 jul 2017 05:05:55 GMT //与资源的更新日期匹配
If-Unmodified-Since：Tue, 03 jul 2017 05:05:55 GMT //与资源的更新日期不匹配
{
    If-Range: '123456'//与资源的实体标识Etag或者更新日期任一匹配成功，作为范围请求，否则返回全体资源
	Range： bytes 5000-10000
}

```

#### Max-forwards

指定该请求可经过的服务器的最大数目。当Max-Forwards为0时，服务器就会立即返回响应。

#### Proxy-Authorization

客户端与代理之间的访问认证。接收到代理服务器发来的认证质询时，客户端会发送包含首部字段Proxy-Authorization的请求。

```
Proxy-Authorization：Basic dskajkfljaskfj····
```

#### Range

用于只获取部分资源的范围请求。接收到Range首部字段时，成功处理会返回206 否则返回200及全部资源。

#### Referer

告知服务器请求的来源URI

#### TE

告知服务器客户端能够处理响应的传输编码方式以及优先级。还可以指定伴随trailer的字段的分块传输编码方式。

```
TE: gzip deflate;q=0.7
——————————————————————
TE: trailers
```

#### User-Agent

包含浏览器和用户代理名称等信息



### 响应首部字段

#### Accept-Ranges 

用了告知客户端服务端能否处理范围请求，可以则指定为bytes 否则指定为none

```
Accept-Ranges: bytes/none
```

#### Age

消息头里包含消息对象在缓存代理中存贮的时长，以秒为单位。.

#### Etag

资源的实体标识，类似于id。当资源更新时，Etag也会更新。

强Etag无论实体发生什么变化，都会变化。

弱Etag只有资源发生了根本变化，才会变化，且在字段值头部附加W/。

#### Location

配合3xx的状态码，提供重定向的URl。

#### Retry-After

告知客户段应该在什么时候或者多久之后再次发送请求，配合状态码503或者3xx一起使用。字段值可以为日期时间格式，也可以为秒

#### Server

服务端上安装的HTTP服务器应用程序的信息。

```
Server: Apache/2.2.6 (unix) PHP/5.2.5
```

#### Vary

是一个HTTP响应头部信息，它决定了对于未来的一个请求头，应该用一个缓存的回复(response)还是向源服务器请求一个新的回复

```
Vary: Accept-Language
//若下一个请求的是相同资源，若Vary指定的Accept-Language在两个请求中相同，则走缓存否则向源服务器重新获取资源。
```



#### WWW-Authenticate

用于HTTP的访问认证。告知客户端适用于访问请求URI所指定资源的认证方案（Basic和Digest）和带参数提示的质询（challenge）状态码401响应肯定带有该首部字段。

```
WWW-Authenticate: Basic realm="Usagidesign Auth"
//realm字段是为了辨别请求URI指定资源所受的保护策略。
```



### 实体首部字段

#### Allow

用于通知客户端，哪些HTTP方法可以用来请求指定资源。服务器接收到不支持的HTTP方法时，会返回405，并把所有支持的方法写入Allow。

```
Allow: HEAD,GET
```

#### Content-Encoding

实体的主体部分的内容编码方式。

```
Content-Encoding： gzip
```

#### Content-Language

告知客户端，实体主体使用的自然语言。

#### Content-Length

实体主体部分的大小，单位字节。实体主体进行内容编码后，无法使用该字段。

#### Content-Location

报文主体部分对应的URI。

#### Content-MD5

由MD5算法生成的值，目的在于检查报文主体在传输过程中是否保持完整，以及确认传输到达。

对报文主体内容执行MD5算法获得128位二进制，再通过Base64写入content-MD5。

接收端收到报文后，对主体执行相同的操作，将结果与Content-MD5进行对比，确保报文的准确性。

#### Content-Range

针对范围请求，告知客户端作为响应返回是目标资源的哪个部分。单位为字节。

```
Content-Range：bytes 5001-10000/10000 //当前发送部分及整体大小
```

#### Content-Type

实体主体内对象的媒体类型。

```
Content-Type： text/html; charset=UTF-8
```

#### Expires

资源失效日期

```
Expires：Tue, 03 jul 2017 05:05:55 GMT
```

#### Last-Modified

最后一次修改的时间

```
Last-Modified: Tue, 03 jul 2017 05:05:55 GMT
```



### Cookie相关的首部字段

#### Set-Cookie

| 属性         | 说明                                                         |
| ------------ | :----------------------------------------------------------- |
| NAME= VALUE  | cookie的名称和值                                             |
| expires=DATE | Cookie的有效期，省略有效期为浏览器会话时间段内。             |
| path=PATH    | 将服务器上的文件目录下作为Cookie的适用对象，否则默认Cookie文档所在的文件目录。 |
| domain=域名  | Cookie适用对象的域名                                         |
| Secure       | 仅在HTTP安全通信时发送Cookie                                 |
| HttpOnly     | 使Cookie不能被Javascript脚本访问,即document.cookie无法读取   |

```
Set-Cookie:name=value; status=enable;expires= ···; domain=···;secure;HttpOnly
```

#### Cookie

告知服务器，客户端想获得Cookie。



## SPDY

在应用层(HTTP)与运输层(TCP)间新加会话层，SPDY以会话层形式运作。

| HTTP应用层 |
| :--------: |
| SPDY会话层 |
| SSL表示层  |
| TCP传输层  |

使用SPDY后，HTTP协议额外获得了以下功能

* 多路复用流
* 赋予请求优先级
* 压缩HTTP首部
* 推送功能
* 服务器提示功能

## WebSocket

建立在HTTP基础上的协议。

特点：

* 服务端推送功能
* 减少通信量（持续连接，首部信息小）。











