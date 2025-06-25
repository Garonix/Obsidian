好的，我们来了！这是非常关键的一步：**为你的应用赋予“记忆”能力，将数据持久化到数据库中。**

我们将完成以下任务：
1.  **安装并运行一个数据库**：我们将使用 **Docker** 来完成这件事。这是现代开发中最简单、最干净的方式。
2.  **引入GORM**：一个非常流行且功能强大的Go ORM（对象关系映射）库。它能让你用Go的结构体来操作数据库表，而不用手写复杂的SQL语句。
3.  **重构代码**：将之前操作内存切片(`slice`)的逻辑，替换为通过GORM操作数据库的逻辑。

---

### 第一部分：准备数据库 (使用Docker)

如果你还没有安装Docker，请先花几分钟安装它。这是每个开发者都应该掌握的工具。
*   **安装Docker Desktop**: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)

安装好并启动Docker后，我们用一行命令就能启动一个MySQL数据库服务。

1.  **打开你的终端**（不是在项目目录里，任何地方都可以）。
2.  **运行以下命令来启动一个MySQL 8的容器**：

    ```bash
    docker run --name mysql-for-go -p 3306:3306 -e MYSQL_ROOT_PASSWORD=your_password -e MYSQL_DATABASE=go_todo_db -d mysql:8
    ```

    让我们分解这个命令：
    *   `docker run`: 运行一个新容器。
    *   `--name mysql-for-go`: 给这个容器起个名字，方便管理。
    *   `-p 3306:3306`: 将你本机的3306端口映射到容器的3306端口。这样你的Go应用（运行在你的电脑上）就能通过`localhost:3306`访问到容器里的MySQL了。
    *   `-e MYSQL_ROOT_PASSWORD=your_password`: 设置MySQL的root用户密码。**请将`your_password`替换为你自己的密码**，并记住它。
    *   `-e MYSQL_DATABASE=go_todo_db`: 在MySQL启动时，自动创建一个名为`go_todo_db`的数据库。
    *   `-d`: 在后台（detached mode）运行容器。
    *   `mysql:8`: 指定使用MySQL版本为8的官方镜像。Docker会自动帮你下载。

    运行后，Docker就会在后台为你准备好一个干净的、配置好的MySQL数据库。你可以通过`docker ps`命令查看它是否正在运行。

---

### 第二部分：代码实战 (引入GORM)

#### 步骤 1: 安装GORM及MySQL驱动

1.  **回到你的 `go-hello-server` 项目目录的终端**。
2.  **安装GORM和MySQL驱动**：

    ```bash
    go get gorm.io/gorm
    go get gorm.io/driver/mysql
    ```
    Go Modules会自动帮你处理好依赖。

#### 步骤 2: 编写新的 `main.go` (GORM版本)

我们将对`main.go`进行一次大重构。这次的代码会更加“工程化”。请仔细阅读注释，理解结构的变化。

将下面的代码完整地复制到你的 `main.go` 文件中：

```go
package main

import (
	"log"
	"net/http"
	"strconv"

	"github.com/gin-gonic/gin"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
)

// Todo 结构体现在是GORM的模型 (gorm.Model)
// gorm.Model 包含了 ID, CreatedAt, UpdatedAt, DeletedAt 字段
type Todo struct {
	gorm.Model
	Title  string `json:"title"`
	Status string `json:"status"`
}

// ---- 全局数据库变量 ----
var DB *gorm.DB

func main() {
	// --- 数据库连接 ---
	// 替换为你自己的数据库密码和名称
	dsn := "root:your_password@tcp(127.0.0.1:3306)/go_todo_db?charset=utf8mb4&parseTime=True&loc=Local"

	var err error
	DB, err = gorm.Open(mysql.Open(dsn), &gorm.Config{})
	if err != nil {
		log.Fatal("无法连接到数据库: ", err)
	}

	// 自动迁移：GORM会自动创建'todos'表
	log.Println("数据库迁移...")
	DB.AutoMigrate(&Todo{})

	// --- Gin 服务器设置 ---
	r := gin.Default()

	api := r.Group("/api")
	{
		api.GET("/todos", getTodos)
		api.GET("/todos/:id", getTodoByID)
		api.POST("/todos", createTodo)
		api.PUT("/todos/:id", updateTodo)
		api.DELETE("/todos/:id", deleteTodo)
	}

	r.Run(":8080")
}

// --- Handler Functions (GORM 版本) ---

// getTodos: 从数据库获取所有待办事项
func getTodos(c *gin.Context) {
	var todos []Todo
	// DB.Find(&todos) 会查询todos表中的所有记录并填充到切片中
	if err := DB.Find(&todos).Error; err != nil {
		c.JSON(http.StatusInternalServerError, gin.H{"error": "查询记录失败"})
		return
	}
	c.JSON(http.StatusOK, todos)
}

// getTodoByID: 从数据库获取单个待办事项
func getTodoByID(c *gin.Context) {
	id := c.Param("id")
	var todo Todo
	// DB.First(&todo, id) 会根据主键查询记录
	if err := DB.First(&todo, id).Error; err != nil {
		// 如果记录未找到，GORM会返回 gorm.ErrRecordNotFound
		if err == gorm.ErrRecordNotFound {
			c.JSON(http.StatusNotFound, gin.H{"error": "未找到待办事项"})
		} else {
			c.JSON(http.StatusInternalServerError, gin.H{"error": "查询记录失败"})
		}
		return
	}
	c.JSON(http.StatusOK, todo)
}

// createTodo: 在数据库中创建新待办事项
func createTodo(c *gin.Context) {
	var input struct {
		Title string `json:"title" binding:"required"`
	}

	if err := c.ShouldBindJSON(&input); err != nil {
		c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		return
	}

	todo := Todo{Title: input.Title, Status: "pending"}
	// DB.Create(&todo) 会将新记录插入数据库
	if err := DB.Create(&todo).Error; err != nil {
		c.JSON(http.StatusInternalServerError, gin.H{"error": "创建记录失败"})
		return
	}

	c.JSON(http.StatusCreated, todo)
}

// updateTodo: 在数据库中更新待办事项
func updateTodo(c *gin.Context) {
	id := c.Param("id")
	var todo Todo
	// 首先，检查记录是否存在
	if err := DB.First(&todo, id).Error; err != nil {
		c.JSON(http.StatusNotFound, gin.H{"error": "未找到待办事项"})
		return
	}

	var input struct {
		Title  string `json:"title"`
		Status string `json:"status"`
	}

	if err := c.ShouldBindJSON(&input); err != nil {
		c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		return
	}

	// DB.Model(&todo).Updates(...) 会更新记录
	if err := DB.Model(&todo).Updates(input).Error; err != nil {
		c.JSON(http.StatusInternalServerError, gin.H{"error": "更新记录失败"})
		return
	}

	c.JSON(http.StatusOK, todo)
}

// deleteTodo: 从数据库中删除待办事项
func deleteTodo(c *gin.Context) {
	id := c.Param("id")
	// DB.Delete(&Todo{}, id) 会根据主键删除记录（软删除）
	result := DB.Delete(&Todo{}, id)
	
	if result.Error != nil {
		c.JSON(http.StatusInternalServerError, gin.H{"error": "删除记录失败"})
		return
	}
	
	if result.RowsAffected == 0 {
		c.JSON(http.StatusNotFound, gin.H{"error": "未找到待办事项"})
		return
	}

	c.Status(http.StatusNoContent)
}

```

#### 步骤 3: 代码关键点讲解

1.  **修改 `Todo` 结构体**:
    *   我们嵌入了 `gorm.Model`。这是一个GORM提供的基础结构体，它自动为我们添加了 `ID` (主键), `CreatedAt`, `UpdatedAt`, `DeletedAt` (用于软删除) 四个字段。这非常方便！

2.  **数据库连接**:
    *   `dsn := "root:your_password@tcp(127.0.0.1:3306)/go_todo_db?..."`: 这是**数据源名称 (Data Source Name)**。你**必须**把 `your_password` 换成你在第一部分Docker命令中设置的密码。
    *   `DB, err = gorm.Open(...)`: 使用GORM和MySQL驱动打开数据库连接。我们将连接句柄 `DB` 存为一个全局变量，方便所有handler函数使用。

3.  **自动迁移 (Auto Migration)**:
    *   `DB.AutoMigrate(&Todo{})`: 这是GORM的魔法之一。它会检查数据库中是否存在一个名为 `todos` 的表（GORM会自动将结构体名 `Todo` 转为复数形式 `todos`）。如果不存在，它会根据 `Todo` 结构体的定义自动创建这张表。

4.  **Handler函数的重构**:
    *   **所有**原来对 `todos` 这个内存切片的操作，现在都换成了对全局变量 `DB` 的操作。
    *   `DB.Find()`: 查找多条记录。
    *   `DB.First()`: 查找单条记录。
    *   `DB.Create()`: 创建记录。
    *   `DB.Model().Updates()`: 更新记录。
    *   `DB.Delete()`: 删除记录。（注意：因为我们用了`gorm.Model`，这默认是**软删除**，即只是在`deleted_at`字段里记录一个时间，数据本身还在。这在很多场景下是更安全的做法。）

---

### 第三部分：运行与测试

1.  **确认Docker中的MySQL容器正在运行**。
2.  **启动服务器**:
    ```bash
    go run main.go
    ```
    观察终端，你会看到 "数据库迁移..." 的日志，表示GORM正在为你建表。

3.  **用Postman重新测试所有API**:
    *   像上一步一样，用Postman逐一测试 `GET`, `POST`, `PUT`, `DELETE` 这几个接口。
    *   你会发现，API的行为和之前完全一样！这就是分层和抽象的威力——我们替换了整个数据层，但对外的API（接口层）没有任何变化。
    *   **最大的不同是**：现在，当你 `Ctrl+C` 停止服务器，然后再重新启动，**你的数据还在！** 因为它们已经被持久化到了Docker容器里的MySQL数据库中。你可以反复创建、删除、更新，数据都会被保留下来。

**恭喜你！你的应用程序现在拥有了持久化存储的能力，这使它成为了一个真正意义上的Web应用。**

**下一步预告：**
我们的后端API已经相当完善了。但是一个完整的项目还缺少一个重要的部分：**前端界面**。下一步，我们将快速创建一个简单的Vue.js前端应用，让用户可以通过浏览器界面来与我们的Go后端API进行交互。准备好进入全栈领域了吗？