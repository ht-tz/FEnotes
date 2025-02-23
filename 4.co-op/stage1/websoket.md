# WebSocket介绍

[toc]



## 1. intro

WebSocket是一种基于HTTP的长链接技术。传统的HTTP协议是一种请求-响应模型，如果浏览器不发送请求，那么服务器无法主动给浏览器推送数据。如果需要定期给浏览器推送数据，例如股票行情，或者不定期给浏览器推送数据，例如在线聊天，基于HTTP协议实现这类需求，只能依靠浏览器的JavaScript定时轮询，效率很低且实时性不高。

因为HTTP本身是基于TCP连接的，所以，WebSocket在HTTP协议的基础上做了一个简单的升级，即建立TCP连接后，浏览器发送请求时，附带几个头：

```
GET /chat HTTP/1.1
Host: www.example.com
Upgrade: websocket
Connection: Upgrade
```

就表示客户端希望升级连接，变成长连接的WebSocket，服务器返回升级成功的响应：

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
```

收到成功响应后表示WebSocket“握手”成功，这样，代表WebSocket的这个TCP连接将不会被服务器关闭，而是一直保持，服务器可随时向浏览器推送消息，浏览器也可随时向服务器推送消息。双方推送的消息既可以是文本消息，也可以是二进制消息，一般来说，绝大部分应用程序会推送基于JSON的文本消息。

-------



WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。WebSockets是一种现代的通信协议，它允许在客户端和服务器之间进行双向通信。无需中断连接也无需额外的 HTTP 请求。与传统的HTTP协议不同，WebSocket提供了一个持久的连接，使得服务器能够主动向客户端推送数据，而不需要客户端发起请求。这种实时通信的能力在许多应用程序中都非常有用，例如在线游戏、实时聊天和股票报价等。WebSockets协议通常基于TCP协议运行，其通信数据是以帧（Frame）的形式发送和接收的。由于其实时性、高效性和可扩展性，WebSockets已经成为现代Web应用程序中的重要组件之一。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。



## 2. WebSockets 的特点和优势

1. 实时性：WebSockets提供了一个持久的连接，可以在客户端和服务器之间进行实时通信，而无需像HTTP请求那样每次都建立新的连接。这使得WebSockets特别适合需要实时更新的应用程序，如股票报价、游戏等。
2. 双向通信：WebSockets不仅可以从服务器向客户端发送数据，也可以从客户端向服务器发送数据。这意味着Web应用程序可以通过WebSockets向服务器发送请求，从而实现更灵活和高效的数据交互。
3. 低延迟：WebSockets是基于TCP协议实现的，可以在客户端和服务器之间提供低延迟的双向数据传输。
4. 轻量级：WebSockets使用的数据帧格式相对较小，比HTTP请求头要小很多。这意味着可以在较小的带宽上实现更高的数据传输效率。
5. 跨域支持：WebSockets可以跨域通信，可以在不同的域之间建立连接，这使得它非常适合实现跨域的Web应用程序。
6. 可扩展性：WebSockets 可以通过协议升级支持多种协议，包括自定义协议，从而具有良好的可扩展性和灵活性。
7. 兼容性：WebSockets 不受浏览器限制，可以在不同的平台和设备上运行，支持多种语言和技术栈，使其适用范围广。
8. 实现简单：WebSockets 的实现相对简单，开发人员可以轻松地将其集成到现有的应用程序中，而无需额外的依赖项。

## 3. WebSockets 和 HTTP 的关系

WebSockets 和 HTTP 都是互联网通信协议，但是它们的设计目的和工作原理不同。

HTTP（Hypertext Transfer Protocol）是一种基于请求和响应模式的协议，通常用于浏览器和服务器之间的单向通信。浏览器通过发送 HTTP 请求来请求服务器上的资源，服务器则通过发送 HTTP 响应来返回资源。这种模式适合传输静态资源，但不适合实时通信和双向通信。

WebSockets 则是一种基于 TCP 的协议，一种双向通信协议，它可以在浏览器和服务器之间建立持久性的连接，实现实时通信和双向通信。WebSockets 的建立需要使用 HTTP 进行握手，但是一旦连接建立成功，就可以直接进行数据传输，不需要像 HTTP 那样每次都建立新的连接。

因此，WebSockets 和 HTTP 的关系是，WebSockets 建立连接时需要使用 HTTP 进行握手，但是连接建立成功后，WebSockets 可以独立于 HTTP 进行数据传输，具有更好的实时性和双向通信能力。同时，WebSockets 也可以与 HTTP 协作实现更为复杂的网络应用。

| 特点           | WebSockets                                               | HTTP                                              |
| -------------- | -------------------------------------------------------- | ------------------------------------------------- |
| 协议类型       | 基于 TCP 的协议                                          | 基于请求-响应的协议                               |
| 连接类型       | 可以建立长连接，支持双向通信                             | 每次请求都需要重新建立连接                        |
| 通信效率       | 传输数据更加高效，支持二进制数据传输                     | 传输数据较慢，只能支持文本数据传输                |
| 支持的数据类型 | 支持传输文本和二进制数据                                 | 主要支持传输文本数据                              |
| 安全性         | 支持 SSL/TLS 加密传输，保证通信的安全性                  | 支持 SSL/TLS 加密传输，但是只能加密请求和响应数据 |
| 使用场景       | 适用于实时通信、推送通知、在线游戏等实时性要求较高的场景 | 适用于获取静态资源、发送表单数据等场景            |

##  WebSocket 握手请求和 HTTP 握手请求的不同点

| 特点               | WebSocket 握手请求                                           | HTTP 握手请求                                                |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 握手地址           | ws:// 或 wss://                                              | http:// 或 https://                                          |
| 握手协议           | WebSocket                                                    | HTTP协议                                                     |
| 是否支持压缩       | 支持                                                         | 支持                                                         |
| 是否支持二进制数据 | 支持                                                         | 支持（但不如 WebSocket 处理二进制数据高效）                  |
| 数据格式           | 帧（Frame）                                                  | 报文（Message）                                              |
| 传输方式           | 双向传输                                                     | 单向传输                                                     |
| 连接保持时间       | 持续连接（不需要每次请求建立连接）                           | 短连接（每次请求需要重新建立连接）                           |
| 适用场景           | 实时数据传输、在线游戏、实时聊天等                           | 请求-响应模式的数据传输                                      |
| 请求头             | `Upgrade`, `Connection`, `Sec-WebSocket-Key`, `Sec-WebSocket-Version`, `Sec-WebSocket-Extensions` | `Host`, `User-Agent`, `Accept`, `Accept-Language`, `Accept-Encoding`, `Connection`, `Upgrade`, `Sec-WebSocket-Version`, `Sec-WebSocket-Key`, `Sec-WebSocket-Extensions` |
| 请求方法           | `GET`                                                        | `GET`, `POST`, `PUT`, `DELETE` 等                            |
| 响应头             | `Upgrade`, `Connection`, `Sec-WebSocket-Accept`, `Sec-WebSocket-Extensions` | `Connection`, `Upgrade`, `Sec-WebSocket-Accept`, `Sec-WebSocket-Extensions` |
| 响应状态码         | `101 Switching Protocols`                                    | `101 Switching Protocols`, `200 OK`, `404 Not Found`, `500 Internal Server Error` 等 |

WebSocket 握手请求和 HTTP 握手请求虽然有不同的特点，但是 WebSocket 握手请求是基于 HTTP 协议实现的，因此在 WebSocket 握手过程中，依然需要 HTTP 协议来进行数据传输。

一个典型的Websocket握手请求如下[[15\]](https://zh.wikipedia.org/wiki/WebSocket#cite_note-15)：

客户端请求：

```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Origin: http://example.com
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
```

服务器回应：

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
Sec-WebSocket-Protocol: chat
```

### 字段说明

- Connection必须设置Upgrade，表示客户端希望连接升级。
- Upgrade字段必须设置Websocket，表示希望升级到Websocket协议。
- Sec-WebSocket-Key是随机的字符串，服务器端会用这些数据来构造出一个SHA-1的信息摘要。把“Sec-WebSocket-Key”加上一个特殊字符串“258EAFA5-E914-47DA-95CA-C5AB0DC85B11”，然后计算[SHA-1](https://zh.wikipedia.org/wiki/SHA-1)摘要，之后进行[Base64](https://zh.wikipedia.org/wiki/Base64)编码，将结果做为“Sec-WebSocket-Accept”头的值，返回给客户端。如此操作，可以尽量避免普通HTTP请求被误认为Websocket协议。
- Sec-WebSocket-Version 表示支持的Websocket版本。RFC6455要求使用的版本是13，之前草案的版本均应当弃用。
- Origin字段是必须的。如果缺少origin字段，WebSocket服务器需要回复HTTP 403 状态码（禁止访问）。[[16\]](https://zh.wikipedia.org/wiki/WebSocket#cite_note-16)
- 其他一些定义在HTTP协议中的字段，如[Cookie](https://zh.wikipedia.org/wiki/Cookie)等，也可以在Websocket中使用。

# WebSockets 安全性

## 1. WebSockets 的安全性问题

WebSocket 协议本身具有一定的安全性，例如使用 TLS/SSL 加密传输数据可以保证通信安全性。但是在实际应用中，仍然存在一些安全性问题，主要包括以下几个方面：

1. 跨站脚本攻击（XSS）：攻击者通过注入恶意脚本，从而获取到 WebSocket 连接的访问权限和数据。解决方案包括使用 Cookie 和 Same-Origin Policy 来限制跨站访问，并且对输入进行过滤和转义。
2. 跨站请求伪造（CSRF）：攻击者通过欺骗用户点击链接或提交表单，从而执行恶意操作，例如发送 WebSocket 消息。解决方案包括使用 CSRF Token 来防止攻击。
3. 中间人攻击（MITM）：攻击者可以窃听和修改 WebSocket 通信的内容。解决方案包括使用 TLS/SSL 加密通信，防止中间人篡改数据。
4. DDos 攻击：攻击者可以通过发送大量的 WebSocket 连接请求和消息，从而导致服务器瘫痪。解决方案包括限制连接数和消息大小，并使用防火墙和负载均衡器来保护服务器。
5. 服务器端漏洞：服务器端存在漏洞可能导致攻击者获取系统权限或者窃取数据。解决方案包括加强服务器的安全性配置，如防火墙、限制访问等，以及对代码进行严格的审计和测试。
6. 无法验证数据来源：WebSocket 在建立连接时，无法验证连接请求的来源是否合法，从而容易受到 IP 欺骗和重放攻击。

## 2. 措施

为了提高 WebSocket 的安全性，可以采取以下措施：

1. 使用 SSL/TLS 协议进行加密传输，防止数据被窃听和篡改。
2. 在 WebSocket 服务器端实施身份验证和授权机制，防止恶意用户伪造 WebSocket 连接。
3. 在 WebSocket 服务器端限制数据传输的大小和频率，防止恶意用户通过 WebSocket 连接进行拒绝服务攻击。
4. 使用随机的 WebSocket URL，防止攻击者进行 IP 欺骗和重放攻击。
5. 在 WebSocket 应用层实施消息签名和加密机制，防止跨站点脚本攻击和跨站点请求伪造攻击。

## 3. 如何使用 SSL/TLS 加密 WebSockets 连接

1. 获得 SSL/TLS 证书：可以使用 OpenSSL 工具生成自签名的 SSL/TLS 证书，例如：

```js
const { execSync } = require('child_process');

// 生成私钥
execSync('openssl genrsa -out key.pem 2048');

// 生成证书请求
const csr = execSync('openssl req -new -key key.pem -out csr.pem');

// 生成自签名证书
execSync('openssl x509 -req -days 365 -in csr.pem -signkey key.pem -out cert.pem');

// 读取证书和私钥
const fs = require('fs');
const cert = fs.readFileSync('cert.pem');
const key = fs.readFileSync('key.pem');
```

通过 `https.createServer` 方法创建一个 HTTPS 服务器，并传入 SSL/TLS 证书和私钥。然后使用 `ws` 模块创建一个 WebSocket 服务器，并将 HTTPS 服务器传入 `WebSocket.Server` 的构造函数中，以实现 SSL/TLS 加密的 WebSocket 服务器。

```js
const https = require('https');
const WebSocket = require('ws');

const server = https.createServer({
  cert: fs.readFileSync('certificate.pem'),
  key: fs.readFileSync('key.pem')
});

const wss = new WebSocket.Server({ server });

wss.on('connection', function connection(ws) {
  console.log('WebSocket connected');
  ws.on('message', function incoming(message) {
    console.log('received: %s', message);
  });
});
```

# WebSockets 应用场景

1. 实时聊天：WebSockets 可以用于实时聊天应用，例如在线客服、实时通讯等。通过 WebSockets，客户端和服务器端可以实现双向通信，实时传递消息。
2. 多人协作：WebSockets 可以用于多人协作应用，例如在线编辑、实时白板等。通过 WebSockets，多个用户可以实时协作，共同编辑或绘制内容。
3. 实时数据推送：WebSockets 可以用于实时数据推送应用，例如股票行情、实时天气等。通过 WebSockets，服务器端可以将实时数据推送给客户端，客户端可以及时获得最新的数据。
4. 游戏应用：WebSockets 可以用于实时游戏应用，例如多人在线游戏、实时竞技等。通过 WebSockets，多个玩家可以实时交互，共同游戏。
5. 物联网应用：WebSockets 可以用于物联网应用，例如智能家居、智能城市等。通过 WebSockets，智能设备可以实时向服务器端发送数据，服务器端可以实时控制智能设备。

# 总结

- WebSockets 的优势和局限性是什么？

1. 实时性：WebSockets 支持双向实时通信，可以在客户端和服务器端之间快速传递消息，实时性更高。
2. 可靠性：WebSockets 支持断线重连，能够保证通信的稳定性和可靠性。
3. 低延迟：WebSockets 使用 TCP 连接，与 HTTP 相比，可以降低通信的延迟。
4. 高效性：WebSockets 使用二进制传输，数据格式更加紧凑，传输效率更高。
5. 兼容性：WebSockets 可以在所有现代浏览器和服务器上使用，并且与 HTTP 和 HTTPS 兼容。

WebSockets 也存在一些局限性：

1. 浏览器支持限制：WebSockets 在早期的浏览器版本中不支持，需要使用 polyfill 或者降级处理。
2. 安全性问题：WebSockets 长时间保持连接，容易被攻击者利用进行 DDos 攻击等行为。需要采取一些安全措施，如限制连接数量、限制数据带宽等。
3. 网络环境限制：WebSockets 需要建立长连接，对一些不稳定的网络环境容易造成负担，如丢包、延迟等。
4. 服务器成本较高：WebSockets 长时间保持连接，需要服务器不断地保持连接和数据推送，对服务器的资源消耗较高。需要合理规划服务器的资源，避免过度消耗。

# 其他...

WebSockets 和其他技术的结合

- 如何使用 WebSockets 和 WebRTC 结合实现实时通信？
- 如何使用 WebSockets 和 Canvas 结合实现绘图应用？
- 如何使用 WebSockets 和 WebGL 结合实现 3D 游戏？

Polyfill 是指用一些代码来模拟浏览器原生的 API 和功能，使得较老版本的浏览器能够使用新的 API 或者新的语言特性。

通常情况下，新的 API 或者新的语言特性只能在最新版本的浏览器中得到支持，这就导致了在较老版本的浏览器中，这些新的功能无法正常使用。为了解决这个问题，开发者们会编写 Polyfill，通过代码模拟这些新的 API 或者语言特性，使得较老版本的浏览器也能够正常使用这些新的功能。

Polyfill 的使用可以大大提高代码的兼容性和可维护性，使得开发者可以更加自由地使用新的 API 或者语言特性，而不必考虑是否兼容。但是，Polyfill 也会增加代码的体积和运行时间，因此需要慎重使用。

Polyfill 的使用可以提高开发效率，同时也可以避免因为浏览器版本差异而产生的兼容性问题。常见的 Polyfill 库包括 Modernizr、polyfill.io、es6-shim 等。











