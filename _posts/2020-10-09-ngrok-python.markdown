---
layout: post
title: Python-接收并打印WebHook请求
date: 2020-10-09 10:30:00 +0900
category: Python
tag: 内网穿透
---
## 什么是WebHook?
WebHook是一个API概念，是微服务API的使用范式之一，也被成为反向API，即：前端不主动发送请求，完全由后端推送。 举个常用例子，比如你的好友发了一
条朋友圈，后端将这条消息推送给所有其他好友的客户端，就是Webhooks的典型场景。

简单来说，WebHook就是一个接收HTTP-POST(或GET，PUT，DELETE)的URL。一个实现了WebHook的API提供商就是在当事件发生的时候会向这个配置好的URL
发送一条信息。与请求-响应式不同，使用WebHook，你可以实时接受到变化。

这是一种对客户机-服务器模式的逆转，在传统方法中，客户端从服务器请求数据，然后服务器提供给客户端数据（客户端是在拉数据）。在Webhook模式下，
服务器更新所需提供的资源，然后自动将其作为更新发送到客户端（服务器是在推数据），客户端不是请求者，而是被动接收方。这种控制关系的反转可以用来
促进许多原本需要在远程服务器上进行更复杂的请求和不断的轮询的通信请求。通过简单地接收资源而不是直接发送请求，我们可以更新远程代码库，轻松地分
配资源，甚至将其集成到现有系统中来根据API的需要来更新端点和相关数据，唯一的缺点是初始建立困难。

#### 1、使用Python的BaseHTTPServer模块接收Post请求
```
import json
from urllib.parse import unquote
from http.server import HTTPServer, BaseHTTPRequestHandler


class RequestHandler(BaseHTTPRequestHandler):
    def _writeheaders(self):
        print(self.path)
        print(self.headers)

    def do_Head(self):
        self._writeheaders()

    def do_GET(self):
        self._writeheaders()
        self.wfile.write(str(self.headers))

    def do_POST(self):
        self._writeheaders()
        data = self.rfile.read(int(self.headers['content-length']))
        data = unquote(str(data, encoding='utf-8'))
        json_obj = json.loads(data)
        print(json_obj)


if __name__ == "__main__":
    addr = ('', 1314)
    server = HTTPServer(addr, RequestHandler)
    server.serve_forever()
```

#### 2、运行上述脚本，启动本地端口1314，利用Ngrok内网穿透，映射外网地址[http://c9a250abdffa.ngrok.io](https://wangxiaoxi.cn/posts/ngrok/)
[点我看Ngrok设置](https://wangxiaoxi.cn/posts/ngrok/)

#### 3、设置WebHook中的Delivery URL为'http://c9a250abdffa.ngrok.io',示例为woocommerce中webhook设置。
![image](/assets/img/cc/webhook-1.png)

#### 4、查看本地端口实时接收的请求数据。
![image](/assets/img/cc/webhook.png)
