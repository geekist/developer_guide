web程序是使用HTTP协议传输的，而HTTP协议是无状态的协议，对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

网络早期最大的问题之一是如何管理状态。简而言之就是服务器无法知道两个请求是否来自同一个浏览器。

会话（Session）跟踪是Web程序中常用的技术，用来跟踪用户的整个会话。常用的会话跟踪技术是Cookie与Session。Cookie通过在客户端记录信息确定用户身份，Session通过在服务器端记录信息确定用户身份。



# 一、浏览器和服务器如何确认身份

HTTP 协议是一种无状态协议，即每次服务端接收到客户端的请求时，都是一个全新的请求，服务器并不知道客户端的历史请求记录；Session 和 Cookie 的主要目的就是为了弥补 HTTP 的无状态特性。

![](./assets/session_1.png)

客户端发送一个http请求到服务器端。

服务器第一次接收到请求时，开辟了一块 Session 空间（创建了Session对象），同时生成一个 sessionId ，并通过响应头的 **Set-Cookie：JSESSIONID=XXXXXXX **命令，向客户端发送要求设置 Cookie 的响应； 客户端收到响应后，在本机客户端设置了一个 **JSESSIONID=XXXXXXX **的 Cookie 信息，该 Cookie 的过期时间为浏览器会话结束；

客户端发起的第二次请求，假如服务器给了set-Cookie，浏览器会自动在请求头中添加cookie

服务器接收请求，分解cookie，验证信息，读取请求头中的 Cookie 信息，获取名称为 JSESSIONID 的值，得到此次请求的 sessionId。核对成功后返回response给客户端

# 二、 Sesson

客户端请求服务端，服务端会为这次请求开辟一块内存空间，这个对象便是 Session 对象，存储结构为 ConcurrentHashMap。Session 弥补了 HTTP 无状态特性，服务器可以利用 Session 存储客户端在同一个会话期间的一些操作记录。

Session 机制是一种服务器端的机制，服务器使用一种类似于散列表的结构（也可能就是使用散列表）来保存信息。

当程序需要为某个客户端的请求创建一个 Session 时，服务器首先检查这个客户端的请求里是否已包含了一个 Session 标识（称为 Session ID），如果已包含则说明以前已经为此客户端创建过 Session，服务器就按照 Session ID 把这个 Session 检索出来使用（检索不到，会新建一个），如果客户端请求不包含 Session ID，则为此客户端创建一个 Session 并且生成一个与此 Session 相关联的 Session ID，Session ID 的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这个 Session ID 将被在本次响应中返回给客户端保存。

实现过程

在服务器端生成全局唯一标识符session_id；
在服务器内存里开辟此session_id对应的数据存储空间；
将session_id作为全局唯一标示符通过cookie发送给客户端；
以后客户端再次访问服务器时会把session_id通过请求头中的cookie发送给服务器；
服务器再通过session_id把此标识符在服务器端的数据取出
如果客户端的浏览器禁用了 Cookie 怎么办？一般这种情况下，会使用一种叫做URL重写的技术来进行会话跟踪，即每次HTTP交互，URL后面都会被附加上

Session 从客户端传输到服务端的方式有三种：

* 通过 Cookie 传输；

* 通过 URL 传输；

* 表单隐藏字段，通过在 <form> 中添加一个隐藏字段，把 Session 传回服务器；


# 三、Cookie

Cookie 是用户浏览器保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。

Cookie 主要用于以下三个方面：

会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）

个性化设置（如用户自定义设置、主题等）

浏览器行为跟踪（如跟踪分析用户行为等）

Cookie的属性：

![](./assets/cookie_1.awebp)

## Domain

Domain 标识指定了哪些主机可以接受 Cookie。如果不指定，默认为当前文档的主机（不包含子域名）。如果指定了 Domain，则一般包含子域名（子域名可以访问父域名的 Cookie）。
例如，如果设置 Domain=mozilla.org，则 Cookie 也包含在子域名中（如 developer.mozilla.org）。
## Path

Path 标识指定了主机下的哪些路径可以接受 Cookie（该 URL 路径必须存在于请求 URL 中）。以字符 %x2F (/) 作为路径分隔符，子路径也会被匹配。
设置 Path=/docs，则以下地址都会匹配：
/docs
/docs/Web/
/docs/Web/HTTP

## Expires/Max-Age
Cookie 的过期时间，过了这个时间之后 Cookie 将会自动删除。
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
Max-Age 的单位是秒。
document.cookie = 'promo_shown=1; Max-Age=2600000; Secure'

## HttpOnly
为避免跨域脚本 (XSS) 攻击，通过 JavaScript 的 Document.cookie API 无法访问带有 HttpOnly 标记的 Cookie，它们只应该发送给服务端。如果包含服务端 Session 信息的 Cookie 不想被客户端 JavaScript 脚本调用，那么就应该为其设置 HttpOnly 标记。
```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

# 四、Json Web Token

Json Web Token 的简称就是 JWT，通常可以称为 Json 令牌。它是RFC 7519 中定义的用于安全的将信息作为 Json 对象进行传输的一种形式。JWT 中存储的信息是经过数字签名的，因此可以被信任和理解。可以使用 HMAC 算法或使用 RSA/ECDSA 的公用/专用密钥对 JWT 进行签名。

使用 JWT 主要用来下面两点

认证(Authorization)：这是使用 JWT 最常见的一种情况，一旦用户登录，后面每个请求都会包含 JWT，从而允许用户访问该令牌所允许的路由、服务和资源。单点登录是当今广泛使用 JWT 的一项功能，因为它的开销很小。

信息交换(Information Exchange)：JWT 是能够安全传输信息的一种方式。通过使用公钥/私钥对 JWT 进行签名认证。此外，由于签名是使用 head 和 payload 计算的，因此你还可以验证内容是否遭到篡改。

JWT 的格式
下面，我们会探讨一下 JWT 的组成和格式是什么

JWT 主要由三部分组成，每个部分用 . 进行分割，各个部分分别是Header、Payload、Signature
总体看来是：

```
header.payload.signature
```

## Header

Header 是 JWT 的标头，它通常由两部分组成：令牌的类型(即 JWT)和使用的 签名算法，例如 HMAC SHA256 或 RSA。

例如
```
{
  "alg": "HS256",
  "typ": "JWT"
}
```

指定类型和签名算法后，Json 块被 Base64Url 编码形成 JWT 的第一部分。

## Payload

Token 的第二部分是 Payload，Payload 中包含一个声明。声明是有关实体（通常是用户）和其他数据的声明。共有三种类型的声明：registered, public 和 private 声明。

registered 声明： 包含一组建议使用的预定义声明，主要包括
ISS	签发人
iss (issuer)	签发人
exp (expiration time)	过期时间
sub (subject)	主题
aud (audience)	受众
nbf (Not Before)	生效时间
iat (Issued At)	签发时间
jti (JWT ID)	编号
public 声明：公共的声明，可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息，但不建议添加敏感信息，因为该部分在客户端可解密。
private 声明：自定义声明，旨在在同意使用它们的各方之间共享信息，既不是注册声明也不是公共声明。
例如
```
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```
然后 payload Json 块会被Base64Url 编码形成 JWT 的第二部分。

## signature

JWT 的第三部分是一个签证信息，这个签证信息由三部分组成

```
header (base64后的)
payload (base64后的)
secret
```

比如我们需要 HMAC SHA256 算法进行签名

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

签名用于验证消息在此过程中没有更改，并且对于使用私钥进行签名的令牌，它还可以验证 JWT 的发送者的真实身份.

## 拼接成完整的token

现在我们把上面的三个由点分隔的 Base64-URL 字符串部分组成在一起，这个字符串可以在 HTML 和 HTTP 环境中轻松传递这些字符串。

下面是一个完整的 JWT 示例，它对 header 和 payload 进行编码，然后使用 signature 进行签名
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```



