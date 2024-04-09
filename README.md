# nginx 负载均衡

> 官方文档解释 [https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server)

在 Nginx 的负载均衡配置中，max_fails 参数用于定义在后端服务器被标记为不可用之前，允许连续失败的最大次数。一旦后端服务器失败达到了指定的次数，Nginx 将暂时停止将请求发送到该服务器，直到经过一定时间或者通过 fail_timeout 参数指定的时间后，再次将其标记为可用。

具体而言，max_fails 参数用于指定在一个时间窗口内允许后端服务器失败的最大次数。当某个后端服务器连续失败的次数达到了 max_fails，Nginx 将认为该服务器不可用，并且会暂时停止将请求发送到该服务器。之后，Nginx 会根据 fail_timeout 参数定义的时间，在该时间段内暂时将该服务器标记为不可用，以防止频繁地尝试失败的服务器。

例如，如果设置了 max_fails 为 3，那么当某个后端服务器连续失败了 3 次之后，Nginx 将暂时停止将请求发送到该服务器，直到经过一段时间后重新尝试。这有助于防止将请求发送到频繁失败的服务器，从而提高了系统的可靠性和稳定性。

```nginx
upstream backend {
    server backend1 max_fails=3 fail_timeout=30s;
    server backend2;
    server backend3;
}

```

在上述示例中，当 backend1 连续失败了 3 次时，Nginx 将暂时停止将请求发送到该服务器，并且会在 30 秒后重新尝试。

## Tips

* 使用软连接： 在 Dockerfile 中可以将 /dev/stdout 软连接到你希望输出日志的位置。例如：

```Dockerfile
RUN ln -sf /dev/stdout /path/to/access.log
```

这将创建一个软连接，将 /path/to/access.log 重定向到 /dev/stdout。

无论使用哪种方法，确保你的应用程序能够正确地将日志输出到 stdout。这样做可以使得 Docker 容器的日志输出能够被 Docker 捕获，并能够通过 Docker 日志管理工具进行查看和处理。
