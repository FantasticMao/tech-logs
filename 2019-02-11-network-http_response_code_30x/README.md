在 HTTP 协议的响应状态码中，30x 表示重定向，意味着当前 HTTP 请求的后续动作需要由用户代理端来执行。原文是「The 3xx (Redirection) class of status code indicates that further action needs to be taken by the user agent in order to fulfill the request.」。

301、302、307 都表示了资源的重定向，服务端需要使用 Location 响应首部来指示客户端使用资源新的 URI。这其中：
* 301 表示永久重定向（Moved Permanently），意味着资源已经永久地被转移到了新的 URI 上，并且客户端的后续请求应该使用新 URI，使用场景例如网站从 HTTP 协议升级到 HTTPS 协议；
* 302 表示临时重定向，即发现了新资源（Found），意味着资源只是临时性地被转移到了新的 URI 上，并且客户端的后续请求应该使用旧 URI；
* 307 也表示临时重定向（Temporary Redirect），与 301、302 不同的是，307 可以确保客户端在重定向时不修改请求的方法。301、301 状态码由于历史原因，客户端在重定向时，可能将 POST 请求改变成 GET 请求；

原文链接：https://tools.ietf.org/html/rfc7231#page-54 。