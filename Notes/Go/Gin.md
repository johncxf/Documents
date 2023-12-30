# Gin

Gin 是一个用 Go (Golang) 编写的 Web 框架。

入门文档：

- https://laravelacademy.org/books/gin-tutorial

## 安装配置

### 环境要求

- Go >= 1.9

### Go Module 安装

```shell
# 1、新建项目目录
$ mkdir gin-demo
# 2、进入项目根目录并初始化 go.mod 文件
$ cd gin-demo & go mod init gin-demo
# 3、下载 gin 框架源码
$ go get -u github.com/gin-gonic/gin
# 4、gin 下载完成，可以下载示例代码
$ curl https://raw.githubusercontent.com/gin-gonic/examples/master/basic/main.go > main.go
#	5、运行
$ go run main.go
```

### 日志设置

设置颜色

```go
// 禁止日志颜色
gin.DisableConsoleColor()

// 强制设置日志颜色
gin.ForceConsoleColor()
```

自定义格式

```go
func main() {
    router := gin.New()
    // LoggerWithFormatter 中间件会将日志写入 gin.DefaultWriter
    // 默认情况下 gin.DefaultWriter 是 os.Stdout
    router.Use(gin.LoggerWithFormatter(func(param gin.LogFormatterParams) string {
        // 自定义日志输出格式
        return fmt.Sprintf("%s - [%s] \"%s %s %s %d %s \"%s\" %s\"\n",
            param.ClientIP,
            param.TimeStamp.Format(time.RFC1123),
            param.Method,
            param.Path,
            param.Request.Proto,
            param.StatusCode,
            param.Latency,
            param.Request.UserAgent(),
            param.ErrorMessage,
        )
    }))
    // 使用 recovery 中间件
    router.Use(gin.Recovery())
    router.GET("/ping", func(c *gin.Context) {
        c.String(200, "pong")
    })
    router.Run(":8080")
}
```

自定义路由格式

```go
func main() {
  r := gin.Default()

  // 默认路由输出格式
  gin.DebugPrintRouteFunc = func(httpMethod, absolutePath, handlerName string, nuHandlers int) {
    log.Printf("endpoint %v %v %v %v\n", httpMethod, absolutePath, handlerName, nuHandlers)
  }

  r.POST("/foo", func(c *gin.Context) {
    c.JSON(http.StatusOK, "foo")
  })

  r.GET("/bar", func(c *gin.Context) {
    c.JSON(http.StatusOK, "bar")
  })

  r.GET("/status", func(c *gin.Context) {
    c.JSON(http.StatusOK, "ok")
  })

  // Listen and Server in http://0.0.0.0:8080
  r.Run()
}
```

日志信息写入文件

```go
// 创建日志文件并设置为 gin.DefaultWriter
f, _ := os.Create("./log/gin.log")
gin.DefaultWriter = io.MultiWriter(f)

// 如果你需要同时写入日志文件和控制台，可以这么做：
gin.DefaultWriter = io.MultiWriter(f, os.Stdout)
```

### 示例代码

```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

var db = make(map[string]string)

func setupRouter() *gin.Engine {
	// 创建一个服务
	ginServer := gin.Default()

	// 加载静态页面
	ginServer.LoadHTMLGlob("templates/*")

	// 重定向
	ginServer.GET("/redirect", func(context *gin.Context) {
		context.Redirect(http.StatusMovedPermanently, "https://www.yiqiesuifeng.cn")
	})

	//404
	ginServer.NoRoute(func(context *gin.Context) {
		context.HTML(http.StatusNotFound, "404.html", nil)
	})

	// hello
	ginServer.GET("/hello", func(context *gin.Context) {
		context.JSON(200, gin.H{
			"code": 0,
			"msg":  "hello gin!",
		})
	})

	// Ping test
	ginServer.GET("/ping", func(c *gin.Context) {
		c.String(http.StatusOK, "pong")
	})

	// Get user value
	ginServer.GET("/user/:name", func(c *gin.Context) {
		user := c.Params.ByName("name")
		value, ok := db[user]
		if ok {
			c.JSON(http.StatusOK, gin.H{"user": user, "value": value})
		} else {
			c.JSON(http.StatusOK, gin.H{"user": user, "status": "no value"})
		}
	})

	// Authorized group (uses gin.BasicAuth() middleware)
	// Same than:
	// authorized := r.Group("/")
	// authorized.Use(gin.BasicAuth(gin.Credentials{
	//	  "foo":  "bar",
	//	  "manu": "123",
	//}))
	authorized := ginServer.Group("/", gin.BasicAuth(gin.Accounts{
		"foo":  "bar", // user:foo password:bar
		"manu": "123", // user:manu password:123
	}))

	/* example curl for /admin with basicauth header
	   Zm9vOmJhcg== is base64("foo:bar")

		curl -X POST \
	  	http://localhost:8080/admin \
	  	-H 'authorization: Basic Zm9vOmJhcg==' \
	  	-H 'content-type: application/json' \
	  	-d '{"value":"bar"}'
	*/
	authorized.POST("admin", func(c *gin.Context) {
		user := c.MustGet(gin.AuthUserKey).(string)

		// Parse JSON
		var json struct {
			Value string `json:"value" binding:"required"`
		}

		if c.Bind(&json) == nil {
			db[user] = json.Value
			c.JSON(http.StatusOK, gin.H{"status": "ok"})
		}
	})

	return ginServer
}

func main() {
	r := setupRouter()

	// 服务器端口
	r.Run(":8088")
}
```

## 使用示例

