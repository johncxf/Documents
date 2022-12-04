# Gin

入门文档：

- https://laravelacademy.org/books/gin-tutorial

## 安装配置

### 环境要求

- Go >= 1.9

### Go Module 安装

```shell
# 1、新建项目目录
$ mkdir gin-demo/basic
# 2、进入项目根目录并初始化 go.mod 文件
$ cd gin-demo/basic & go mod init gin-demo/basic
# 3、下载 gin 框架源码
$ get -u github.com/gin-gonic/gin
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

