- **`curl [URL]`**
  - 作用：获取指定 URL 的内容。
  - 示例：`curl http://example.com`

- **`curl -o filename [URL]`**
  - 作用：将输出保存到文件中。
  - 示例：`curl -o example.html http://example.com`

- **`curl -O [URL]`**
  - 作用：将远程文件的内容下载到本地文件中，并保持远程文件的名称。
  - 示例：`curl -O http://example.com/file.zip`

- **`curl -d "data" [URL]`**
  - 作用：向服务器发送 POST 请求的数据。
  - 示例：`curl -d "param1=value1&param2=value2" http://example.com`

- **`curl -X request [URL]`**
  - 作用：指定请求的类型（如 GET, POST, PUT, DELETE 等）。
  - 示例：`curl -X POST http://example.com`

- **`curl -s [URL]`** 或 **`curl --silent [URL]`**
  - 作用：静默模式，不显示进度条和错误消息。
  - 示例：`curl -s http://example.com`

- **`curl -v [URL]`** 或 **`curl --verbose [URL]`**
  - 作用：详细模式，显示通信详情。
  - 示例：`curl -v http://example.com`

- **`curl -H "header" [URL]`**
  - 作用：添加自定义的 HTTP 头。
  - 示例：`curl -H "X-My-Header: 123" http://example.com`

- **`curl -b "cookie" [URL]`**
  - 作用：携带指定的 cookie 发送请求。
  - 示例：`curl -b "session=abc123" http://example.com`

- **`curl -c "cookie" [URL]`**
  - 作用：将服务器返回的 cookie 保存到指定的文件中。
  - 示例：`curl -c cookies.txt http://example.com`

- **`curl -u username:password [URL]`**
  - 作用：使用 HTTP 基本认证。
  - 示例：`curl -u user:pass http://example.com`

- **`curl -A "agent" [URL]`**
  - 作用：设置用户代理字符串。
  - 示例：`curl -A "Mozilla/5.0" http://example.com`

- **`curl -I [URL]`** 或 **`curl --head [URL]`**
  - 作用：只获取 HTTP 头信息。
  - 示例：`curl -I http://example.com`

- **`curl -L [URL]`**
  - 作用：curl 重定向到新位置，可结合 `-O` 使用。
  - 示例：`curl -L http://example.com`

- **`curl --limit-rate rate [URL]`**
  - 作用：限制下载或上传的速度。
  - 示例：`curl --limit-rate 100k http://example.com`