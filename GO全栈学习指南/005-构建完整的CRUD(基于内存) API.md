现在我们进入最激动人心的部分——**亲手构建一个完整的CRUD API**。

CRUD 代表了四种基本的数据操作：
*   **C**reate (创建)
*   **R**ead (读取)
*   **U**pdate (更新)
*   **D**elete (删除)

掌握了CRUD，你就掌握了构建绝大多数Web应用的核心能力。我们将以一个经典的“待办事项列表 (To-Do List)”为例。

**目标**：创建一个管理待办事项的API。

**特别注意**：在这一步，我们**暂时不使用数据库**。我们会用一个Go的切片（Slice）在内存中模拟数据库。这能让我们完全专注于API的逻辑，而不被数据库的配置分心。这是学习过程中的一个重要技巧：**分离关注点**。

---

### 第一部分：API 设计 (思考先行)

在写代码之前，先设计好我们的API接口。这是一个非常重要的专业习惯。

**数据模型 (Todo Struct):**
一个待办事项应该有什么属性？
*   `ID`: 唯一标识符
*   `Title`: 标题内容
*   `Status`: 状态 (比如 "pending" 或 "completed")

**API 路由 (Endpoints):**
我们需要哪些接口来操作这些待办事项？

| HTTP 方法 | URL 路径           | 描述                     |
| :-------- | :----------------- | :----------------------- |
| `GET`     | `/api/todos`       | 获取所有待-办事项        |
| `GET`     | `/api/todos/:id`   | 获取单个待办事项         |
| `POST`    | `/api/todos`       | 创建一个新的待办事项     |
| `PUT`     | `/api/todos/:id`   | 更新一个已有的待办事项   |
| `DELETE`  | `/api/todos/:id`   | 删除一个待办事项         |

这里的 `:id` 是一个**路径参数 (Path Parameter)**，它是一个占位符，代表了待办事项的唯一ID。Gin让处理这种动态路由变得非常简单。

---

### 第二部分：代码实战

#### 步骤 1: 准备工作

1.  在 `go-hello-server` 目录下，确保你已经停止了之前的服务器 (`Ctrl+C`)。
2.  我们将重写 `main.go`。你可以先清空它。

#### 步骤 2: 编写代码

将下面的完整代码复制到你的 `main.go` 文件中。代码里包含了详细的注释，请仔细阅读。

```go
package main

import (
	"net/http"
	"strconv" // 用于将字符串ID转换为整数

	"github.com/gin-gonic/gin"
)

// Todo 定义了待办事项的结构体
type Todo struct {
	ID     int    `json:"id"`
	Title  string `json:"title"`
	Status string `json:"status"` // "pending", "completed"
}

// ---- 模拟数据库 ----
// 使用一个切片来在内存中存储待办事项
var todos = []Todo{
	{ID: 1, Title: "学习 Go 语言", Status: "completed"},
	{ID: 2, Title: "学习 Gin 框架", Status: "completed"},
	{ID: 3, Title: "构建 CRUD API", Status: "pending"},
}
// 用于生成新的ID，简单自增
var nextTodoID = 4
// --------------------

func main() {
	r := gin.Default()

	// 使用路由组，方便管理API版本，这是一个好习惯
	api := r.Group("/api")
	{
		// GET /api/todos - 获取所有待办事项 (Read)
		api.GET("/todos", getTodos)

		// GET /api/todos/:id - 获取单个待办事项 (Read)
		api.GET("/todos/:id", getTodoByID)

		// POST /api/todos - 创建新待办事项 (Create)
		api.POST("/todos", createTodo)

		// PUT /api/todos/:id - 更新待办事项 (Update)
		api.PUT("/todos/:id", updateTodo)

		// DELETE /api/todos/:id - 删除待办事项 (Delete)
		api.DELETE("/todos/:id", deleteTodo)
	}

	r.Run(":8080")
}

// --- Handler Functions ---

// getTodos: 处理 GET /api/todos 请求
func getTodos(c *gin.Context) {
	c.JSON(http.StatusOK, todos)
}

// getTodoByID: 处理 GET /api/todos/:id 请求
func getTodoByID(c *gin.Context) {
	// c.Param("id") 从URL路径中获取参数
	idStr := c.Param("id")
	id, err := strconv.Atoi(idStr) // 将字符串ID转为整数
	if err != nil {
		c.JSON(http.StatusBadRequest, gin.H{"error": "无效的ID"})
		return
	}

	// 遍历切片，查找匹配的todo
	for _, todo := range todos {
		if todo.ID == id {
			c.JSON(http.StatusOK, todo)
			return
		}
	}

	// 如果循环结束还没找到，说明不存在
	c.JSON(http.StatusNotFound, gin.H{"error": "未找到待办事项"})
}

// createTodo: 处理 POST /api/todos 请求
func createTodo(c *gin.Context) {
	var newTodo Todo

	// c.ShouldBindJSON(&newTodo) 将请求体中的JSON绑定到newTodo结构体上
	// 这是一个非常强大的功能！
	if err := c.ShouldBindJSON(&newTodo); err != nil {
		c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		return
	}

	// 分配一个新的ID
	newTodo.ID = nextTodoID
	nextTodoID++
    // 默认新任务状态为 pending
    newTodo.Status = "pending"

	// 将新的todo添加到切片中
	todos = append(todos, newTodo)

	// 返回新创建的todo，状态码201表示资源已创建
	c.JSON(http.StatusCreated, newTodo)
}

// updateTodo: 处理 PUT /api/todos/:id 请求
func updateTodo(c *gin.Context) {
	// 获取ID
	idStr := c.Param("id")
	id, err := strconv.Atoi(idStr)
	if err != nil {
		c.JSON(http.StatusBadRequest, gin.H{"error": "无效的ID"})
		return
	}

	// 获取请求体中的更新数据
	var updatedTodo Todo
	if err := c.ShouldBindJSON(&updatedTodo); err != nil {
		c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		return
	}

	// 在切片中查找并更新
	for i, todo := range todos {
		if todo.ID == id {
			// 更新找到的todo
			todos[i].Title = updatedTodo.Title
			todos[i].Status = updatedTodo.Status
			c.JSON(http.StatusOK, todos[i])
			return
		}
	}

	c.JSON(http.StatusNotFound, gin.H{"error": "未找到待办事项"})
}

// deleteTodo: 处理 DELETE /api/todos/:id 请求
func deleteTodo(c *gin.Context) {
	// 获取ID
	idStr := c.Param("id")
	id, err := strconv.Atoi(idStr)
	if err != nil {
		c.JSON(http.StatusBadRequest, gin.H{"error": "无效的ID"})
		return
	}

	// 在切片中查找并删除
	for i, todo := range todos {
		if todo.ID == id {
			// 从切片中删除元素
			// 将第i个元素之后的部分，拼接到第i个元素之前的部分
			todos = append(todos[:i], todos[i+1:]...)
			// 返回204 No Content表示成功删除，但没有内容返回
			c.Status(http.StatusNoContent)
			return
		}
	}

	c.JSON(http.StatusNotFound, gin.H{"error": "未找到待办事项"})
}

```

### 第三部分：运行与测试 (使用Postman)

这次，浏览器只能帮我们测试GET请求。要完整测试所有CRUD操作，**必须使用Postman**。

1.  **启动服务器**:
    ```bash
    go run main.go
    ```

2.  **用Postman逐一测试你的API**:

    *   **【Read All】获取所有待办事项**
        *   方法: `GET`
        *   URL: `http://localhost:8080/api/todos`
        *   预期结果: 返回一个包含3个初始待办事项的JSON数组。

    *   **【Create】创建新待-办事项**
        *   方法: `POST`
        *   URL: `http://localhost:8080/api/todos`
        *   点击 `Body` -> `raw` -> 选择 `JSON`
        *   在请求体中输入 (注意，我们不需要提供ID和Status，后端会自动处理):
            ```json
            {
                "title": "用 Postman 测试 API"
            }
            ```
        *   预期结果: 返回新创建的todo，ID为4，Status为"pending"。

    *   **【Read One】获取单个待办事项**
        *   方法: `GET`
        *   URL: `http://localhost:8080/api/todos/3`
        *   预期结果: 返回ID为3的待办事项 (`"构建 CRUD API"`)。

    *   **【Update】更新待办事项**
        *   方法: `PUT`
        *   URL: `http://localhost:8080/api/todos/3`
        *   点击 `Body` -> `raw` -> 选择 `JSON`
        *   在请求体中输入:
            ```json
            {
                "title": "构建一个超棒的 CRUD API",
                "status": "completed"
            }
            ```
        *   预期结果: 返回更新后的todo，其title和status已改变。你可以再次GET这个ID的todo来验证。

    *   **【Delete】删除待办事项**
        *   方法: `DELETE`
        *   URL: `http://localhost:8080/api/todos/2`
        *   预期结果: 状态码返回 `204 No Content`，Body为空。

    *   **验证删除**
        *   再次执行 **【Read All】** (`GET /api/todos`)。
        *   预期结果: 返回的数组中不再包含ID为2的待办事项。

**重要提醒**：因为我们现在用的是内存存储，所以**只要你重启服务器 (`Ctrl+C` 再 `go run`)，所有的数据都会恢复到初始状态**。这在开发阶段是好事，方便我们反复测试。

**恭喜你！你已经从零开始，亲手构建并测试了一个功能完整的API服务。** 这是从“写代码”到“做工程”质的飞跃。

**下一步预告：**
我们的API现在已经很棒了，但它有个“致命”的缺点：数据无法持久化。下一步，我们将引入真正的数据库（比如MySQL或PostgreSQL），并使用一个名为 **GORM** 的库（Object-Relational Mapper），将我们的内存操作无缝切换到数据库操作。准备好让你的应用拥有“记忆”了吗？