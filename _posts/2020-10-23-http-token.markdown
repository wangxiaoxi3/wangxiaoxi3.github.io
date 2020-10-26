---
layout: post
title: 前后端常见的鉴权方式
date: 2020-10-23 11:30:00 +0900
category: HTTP
tag: 鉴权方式
---

常用的鉴权有四种：
* HTTP Basic Authentication
* session-cookie
* Token 验证
* OAuth(开放授权)

### 一、HTTP Basic Authentication
这种授权方式是浏览器遵守http协议实现的基本授权方式,HTTP协议进行通信的过程中，HTTP协议定义了基本认证认证允许HTTP服务器对客户端进行用户身份证的方法。\
**效果：**\
客户端未未认证的时候，会弹出用户名密码输入框，这个时候请求时属于pending状态，这个时候其实服务当用户输入用户名密码的时候客户端会再次发送带
Authentication头的请求。

**Authentication认证过程：**

* 1、客户端向服务器请求数据，请求的内容可能是一个网页或者是一个ajax异步请求，此时，假设客户端尚未被验证，则客户端提供如下请求至服务器:\
  Get /index.html HTTP/1.0\
  Host:www.google.com

* 2、服务器向客户端发送验证请求代码401,（WWW-Authenticate: Basic realm=”google.com”这句话是关键，如果没有客户端不会弹出用户名和密码输入
界面）服务器返回的数据大抵如下：\
  HTTP/1.0 401 Unauthorised\
  Server: SokEvo/1.0\
  WWW-Authenticate: Basic realm=”google.com”\
  Content-Type: text/html\
  Content-Length: xxx

* 3、当符合http1.0或1.1规范的客户端（如IE，FIREFOX）收到401返回值时，将自动弹出一个登录窗口，要求用户输入用户名和密码。

* 4、用户输入用户名和密码后，将用户名及密码以BASE64加密方式加密，并将密文放入前一条请求信息中，则客户端发送的第一条请求信息则变成如下内容：\
  Get /index.html HTTP/1.0\
  Host:www.google.com\
  Authorization: Basic d2FuZzp3YW5n

* 5、服务器收到上述请求信息后，将Authorization字段后的用户信息取出、解密，将解密后的用户名及密码与用户数据库进行比较验证，如用户名及密码正确，
服务器则根据请求，将所请求资源发送给客户端。

### 二、session-cookie
利用服务器端的session(会话)和浏览器端的cookie来实现前后端的认证，由于http请求时是无状态的，服务器正常情况下是不知道当前请求之前有没有来过，
这个时候我们如果要记录状态，就需要在服务器端创建一个会话(session),将同一个客户端的请求都维护在各自的会话中，每当请求到达服务器端的时候，
先去查一下该客户端有没有在服务器端创建session，如果有则已经认证成功了，否则就没有认证。

**session-cookie认证过程：**：
* 1、服务器在接受客户端首次访问时在服务器端创建seesion，然后保存seesion(我们可以将seesion保存在内存中，也可以保存在redis中，推荐使用后者)，然后给这个session生成一个唯一的标识字符串,然后在响应头中种下这个唯一标识字符串。

* 2、签名。这一步只是对sid进行加密处理，服务端会根据这个secret密钥进行解密。（非必需步骤）

* 3、浏览器中收到请求响应的时候会解析响应头，然后将sid保存在本地cookie中，浏览器在下次http请求de 请求头中会带上该域名下的cookie信息，

* 4、服务器在接受客户端请求时会去解析请求头cookie中的sid，然后根据这个sid去找服务器端保存的该客户端的session，然后判断该请求是否合法。

![image](/assets/img/cc/cookie.png)

### 三、Token 验证
使用基于 Token 的身份验证方法，大概的流程是这样的：
* 1、客户端使用用户名跟密码请求登录
* 2、服务端收到请求，去验证用户名与密码
* 3、验证成功后，服务端会签发一个 Token，再把这个 Token 发送给客户端
* 4、客户端收到 Token 以后可以把它存储起来，比如放在 Cookie 里或者 Local Storage 里
* 5、客户端每次向服务端请求资源的时候需要带着服务端签发的 Token
* 6、服务端收到请求，然后去验证客户端请求里面带着的 Token，如果验证成功，就向客户端返回请求的数据

JWT是Auth0提出的通过对JSON进行加密签名来实现授权验证的方案，就是登陆成功后将相关信息组成json对象，然后对这个对象进行某中方式的加密，
返回给客户端，客户端在下次请求时带上这个token，服务端再收到请求时校验token合法性，其实也就是在校验请求的合法性。

JWT对象通常由三部分构成：

* 1、Headers：包括类别（type）、加密算法（alg）
```
    {
      "alg": "HS256",
      "typ": "JWT"
    }
```
* 2、Claims：包括需要传递的用户信息
```
    {
      "sub": "1234567890",
      "name": "John Doe",
      "admin": true
    }
```
* 3、Signature：根据alg算法与私有秘钥进行加密得到的签名字串，这一段是最重要的敏感信息，只能在服务端解密
```
HMACSHA256(
    base64UrlEncode(Headers) + "." +
    base64UrlEncode(Claims),
    SECREATE_KEY
)
```
编码之后的JWT看起来是这样的一串字符：\
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ


### 四、OAuth(开放授权)
OAuth（开放授权）是一个开放标准，允许用户授权第三方网站访问他们存储在另外的服务提供者上的信息，而不需要将用户名和密码提供给第三方网站或分享
他们数据的所有内容，为了保护用户数据的安全和隐私，第三方网站访问用户数据前都需要显式的向用户征求授权。
我们常见的提供OAuth认证服务的厂商有支付宝，QQ,微信。

OAuth协议有1.0和2.0两个版本。相比较1.0版，2.0版整个授权验证流程更简单更安全，也是目前最主要的用户身份验证和授权方式。

**Auth2.0认证流程：**
* 1、向用户请求授权，现在很多的网站在登陆的时候都有第三方登陆的入口，当我们点击等第三方入口时，第三方授权服务会引导我们进入第三方登陆授权页面。
```
https://graph.qq.com/oauth2.0/show?which=Login&display=pc&response_type=code&client_id=100270989&redirect_uri=https%3A%
2F%2Fpassport.csdn.net%2Faccount%2Flogin%3Foauth_provider%3DQQProvider&state=test
```
* 2、返回用户凭证（code），当用户点击授权并登陆后，授权服务器将生成一个用户凭证（code）。这个用户凭证会附加在重定向的地址redirect_uri的后面。
```
https://passport.csdn.net/account/login?code=9e3efa6cea739f9aaab2&state=XXX
```
* 3、请求服务器授权: 获取Access Token，用他来向服务器获取用户信息等资源。

* 4、授权服务器同意授权后，返回一个资源访问的凭证（Access Token）。

* 5、第三方应用通过第四步的凭证（Access Token）向资源服务器请求相关资源。

* 6、资源服务器验证凭证（Access Token）通过后，将第三方应用请求的资源返回。


---
参考文档：[https://blog.csdn.net/wang839305939/article/details/78713124](https://blog.csdn.net/wang839305939/article/details/78713124)
