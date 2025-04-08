- **`screen`**
  - 作用：启动一个新的会话。
  - 示例：`screen`

- **`screen -S <会话名>`**
  - 作用：创建一个带名称的会话。
  - 示例：`screen -S mysession`

- **`screen -ls`**
  - 作用：列出所有当前的会话。
  - 示例：`screen -ls`

- **`screen -r <会话ID或名称>`**
  - 作用：恢复一个已断开的会话。
  - 示例：`screen -r mysession`

- **`screen -d <会话ID或名称>`**
  - 作用：断开指定会话，但不退出会话。
  - 示例：`screen -d mysession`

- **`screen -d -r <会话ID或名称>`**
  - 作用：强制断开并重新连接到指定会话。
  - 示例：`screen -d -r mysession`

- **`screen -X -S <会话名> quit`**
  - 作用：强制关闭指定名称的会话。
  - 示例：`screen -X -S mysession quit`

- **`screen -X -S <会话ID> quit`**
  - 作用：强制关闭指定 ID 的会话。
  - 示例：`screen -X -S 12345 quit`