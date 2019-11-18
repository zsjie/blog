# nginx 日志格式

[nginx 日志模块官方文档][1]

## 日志变量

nginx 的日志格式可以通过以下日志变量定义：

`$remote_addr`

IP 地址，从这里可以看到请求从哪个 IP 发出

`$remote_user`

远程用户

`$requst`

请求，包括请求方法和 URL

`$body_bytes_sent`

发送给客户端请求体的 byte 数量

`$bytes_sent`

发送给客户端的 byte 数量

`$connection`

连接的序列号

`$connection_requests`

通过一个连接发送的当前请求数（the current number of requests made through a connection）

`$msec`

 写入日志时的时间，单位为秒，精确毫秒。
 
 `$pipe`
 
“p” if request was pipelined, “.” otherwise

`$request_length`

请求的长度，包括 request line，请求头和请求体

`$request_time`

处理请求所花时间，从接受到第一个 byte 到最后一个 byte 发送给客户端

`$status`

响应状态码

`$time_iso8601`

ISO 8601 标准格式的当地时间

`$time_local`

普通格式的当地时间

`$http_referer`

http referer，从这里可以看到请求从哪个域名发出

`$http_user_agent`

客户端 user agent

### 默认的日志格式 `combined`

    log_format combined '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent"';

举个例子，打印出来的日志会长这样：

```
101.33.122.22 - - [19/Nov/2019:00:44:17 +0800] "POST /api/path HTTP/1.1" 200 2 "-" "Mozilla/4.0"
```

[1]: http://nginx.org/en/docs/http/ngx_http_log_module.html#access_log
