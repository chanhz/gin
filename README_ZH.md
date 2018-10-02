# Gin Web Framework

<img align="right" width="159px" src="https://raw.githubusercontent.com/gin-gonic/logo/master/color.png">

[![Build Status](https://travis-ci.org/gin-gonic/gin.svg)](https://travis-ci.org/gin-gonic/gin)
[![codecov](https://codecov.io/gh/gin-gonic/gin/branch/master/graph/badge.svg)](https://codecov.io/gh/gin-gonic/gin)
[![Go Report Card](https://goreportcard.com/badge/github.com/gin-gonic/gin)](https://goreportcard.com/report/github.com/gin-gonic/gin)
[![GoDoc](https://godoc.org/github.com/gin-gonic/gin?status.svg)](https://godoc.org/github.com/gin-gonic/gin)
[![Join the chat at https://gitter.im/gin-gonic/gin](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/gin-gonic/gin?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Sourcegraph](https://sourcegraph.com/github.com/gin-gonic/gin/-/badge.svg)](https://sourcegraph.com/github.com/gin-gonic/gin?badge)
[![Open Source Helpers](https://www.codetriage.com/gin-gonic/gin/badges/users.svg)](https://www.codetriage.com/gin-gonic/gin)
[![Release](https://img.shields.io/github/release/gin-gonic/gin.svg?style=flat-square)](https://github.com/gin-gonic/gin/releases)

Gin 是一个高性能的 Golang WEB框架，拥有 [martini](https://github.com/go-martini/martini) 一样的API,但因采用了高性能的 Golang 路由[httprouter](https://github.com/julienschmidt/httprouter)，Gin 比 matini 快将近40倍！如果你想用优雅的代码实现高性能的WEB服务, Gin是一个很好的选择。

![Gin console logger](https://gin-gonic.github.io/gin/other/console.png)

## Contents

- [安装](#installation)
- [运行条件](#prerequisite)
- [快速开始](#quick-start)
- [基准测试](#benchmarks)
- [Gin v1.stable](#gin-v1-stable)
- [JSON解析:jsoniter](#build-with-jsoniter)
- [快速上手：API Examples](#api-examples)
    - [处理REST风格的请求](#using-get-post-put-patch-delete-and-options)
    - [处理path里的参数](#parameters-in-path)
    - [处理URL里的查询参数](#querystring-parameters)
    - [处理表单请求](#multiparturlencoded-form)
    - [处理query + POST表单的例子](#another-example-query--post-form)
    - [处理 Map querystring or  post表单参数](#map-as-querystring-or-postform-parameters)
    - [文件上传](#upload-files)
    - [路由分组](#grouping-routes)
    - [不带默认middleware的Gin](#blank-gin-without-middleware-by-default)
    - [自定义middleware](#using-middleware)
    - [日志输出](#how-to-write-log-file)
    - [Model绑定和校验](#model-binding-and-validation)
    - [通用Validators](#custom-validators)
    - [如何仅绑定URL中的参数](#only-bind-query-string)
    - [绑定URL参数或data中的json串](#bind-query-string-or-post-data)
    - [绑定checkboxes](#bind-html-checkboxes)
    - [form形式的多值绑定](#multiparturlencoded-binding)
    - [XML, JSON, YAML and ProtoBuf 渲染](#xml-json-yaml-and-protobuf-rendering)
    - [JSONP 渲染](#jsonp)
    - [静态文件服务](#serving-static-files)
    - [data from reader 服务](#serving-data-from-reader)
    - [带模板的HTML rendering](#html-rendering)
    - [Multitemplate](#multitemplate)
    - [重定向](#redirects)
    - [自定义Middleware](#custom-middleware)
    - [权限控制：使用 BasicAuth() middleware](#using-basicauth-middleware)
    - [ middleware 中的 Goroutines ](#goroutines-inside-a-middleware)
    - [自定义 HTTP 配置](#custom-http-configuration)
    - [在 Gin 中集成 Let's Encrypt](#support-lets-encrypt)
    - [利用 Gin 实现多种服务](#run-multiple-service-using-gin)
    - [更优雅的启停](#graceful-restart-or-stop)
    - [用 templates 构建单一二进制文件](#build-a-single-binary-with-templates)
    - [用带 tag 的 struct 绑定请求表单中的数据](#bind-form-data-request-with-custom-struct)
    - [尝试使用不同的 struct 绑定请求body ](#try-to-bind-body-into-different-structs)
    - [http2 服务器推送](#http2-server-push)
    - [自定义路由 log 格式](#define-format-for-the-log-of-routes)
- [测试(testings)](#testing)
- [谁在使用 Gin](#users)

## 安装

在安装 gin 之前，要先安装 go 然后在环境变量中设置 $GOROOT 和$GOPATH.

1. 下载安装

```sh
$ go get -u github.com/gin-gonic/gin
```

2. 在项目代码中 import:

```go
import "github.com/gin-gonic/gin"
```

3. （可选）如果你的项目打算引用 http 包，例如 `http.StatusOK` 这种常量，记得导入 `net/http` .

```go
import "net/http"
```

### 使用包依赖工具(vender tool)导入，如 [Govendor](https://github.com/kardianos/govendor)

1. `go get` govendor

```sh
$ go get github.com/kardianos/govendor
```
2. 创建项目

```sh
$ mkdir -p $GOPATH/src/github.com/myusername/project && cd "$_"
```

3. 使用 `govender` 初始化项目，添加 gin 到项目

```sh
$ govendor init
$ govendor fetch github.com/gin-gonic/gin@v1.3
```

4. Copy 一份开始模板到你的项目中

```sh
$ curl https://raw.githubusercontent.com/gin-gonic/gin/master/examples/basic/main.go > main.go
```

5. 运行

```sh
$ go run main.go
```

## 运行条件

Gin 依赖的 Go 版本： Go 1.6 或更高的版本，很快 Gin 将会依赖 Go 1.7 或更高层的版本。


##  快速开始

```sh
$ vim example.go
```

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "pong",
		})
	})
	r.Run() // 启动服务并监听 0.0.0.0:8080
}
```

```
# run example.go ,在浏览器访问 0.0.0.0:8080/ping 
$ go run example.go
```

## 性能基准测试(Benchmarks)

Gin 使用常规版本的 [HttpRouter] (https://github.com/julienschmidt/httprouter) 

[所有 benchmarks](/BENCHMARKS.md)

Benchmark name                              | (1)        | (2)         | (3) 		    | (4)
--------------------------------------------|-----------:|------------:|-----------:|---------:
**BenchmarkGin_GithubAll**                  | **30000**  |  **48375**  |     **0**  |   **0**
BenchmarkAce_GithubAll                      |   10000    |   134059    |   13792    |   167
BenchmarkBear_GithubAll                     |    5000    |   534445    |   86448    |   943
BenchmarkBeego_GithubAll                    |    3000    |   592444    |   74705    |   812
BenchmarkBone_GithubAll                     |     200    |  6957308    |  698784    |  8453
BenchmarkDenco_GithubAll                    |   10000    |   158819    |   20224    |   167
BenchmarkEcho_GithubAll                     |   10000    |   154700    |    6496    |   203
BenchmarkGocraftWeb_GithubAll               |    3000    |   570806    |  131656    |  1686
BenchmarkGoji_GithubAll                     |    2000    |   818034    |   56112    |   334
BenchmarkGojiv2_GithubAll                   |    2000    |  1213973    |  274768    |  3712
BenchmarkGoJsonRest_GithubAll               |    2000    |   785796    |  134371    |  2737
BenchmarkGoRestful_GithubAll                |     300    |  5238188    |  689672    |  4519
BenchmarkGorillaMux_GithubAll               |     100    | 10257726    |  211840    |  2272
BenchmarkHttpRouter_GithubAll               |   20000    |   105414    |   13792    |   167
BenchmarkHttpTreeMux_GithubAll              |   10000    |   319934    |   65856    |   671
BenchmarkKocha_GithubAll                    |   10000    |   209442    |   23304    |   843
BenchmarkLARS_GithubAll                     |   20000    |    62565    |       0    |     0
BenchmarkMacaron_GithubAll                  |    2000    |  1161270    |  204194    |  2000
BenchmarkMartini_GithubAll                  |     200    |  9991713    |  226549    |  2325
BenchmarkPat_GithubAll                      |     200    |  5590793    | 1499568    | 27435
BenchmarkPossum_GithubAll                   |   10000    |   319768    |   84448    |   609
BenchmarkR2router_GithubAll                 |   10000    |   305134    |   77328    |   979
BenchmarkRivet_GithubAll                    |   10000    |   132134    |   16272    |   167
BenchmarkTango_GithubAll                    |    3000    |   552754    |   63826    |  1618
BenchmarkTigerTonic_GithubAll               |    1000    |  1439483    |  239104    |  5374
BenchmarkTraffic_GithubAll                  |     100    | 11383067    | 2659329    | 21848
BenchmarkVulcan_GithubAll                   |    5000    |   394253    |   19894    |   609

- (1): Total Repetitions achieved in constant time, higher means more confident result
- (2): Single Repetition Duration (ns/op), lower is better
- (3): Heap Memory (B/op), lower is better
- (4): Average Allocations per Repetition (allocs/op), lower is better

## Gin v1. stable

- [x] Zero allocation router.
- [x] Still the fastest http router and framework. From routing to writing.
- [x] Complete suite of unit tests
- [x] Battle tested
- [x] API frozen, new releases will not break your code.

## Build with [jsoniter](https://github.com/json-iterator/go)

Gin 使用 `encoding/json` 作为默认的 json package , 但也可以使用例如 [jsoniter](https://github.com/json-iterator/go) 的包作为构建的选项

```sh
$ go build -tags=jsoniter .
```

## API Examples

### Using GET, POST, PUT, PATCH, DELETE and OPTIONS

```go
func main() {
	// 禁用 Console Color
	// gin.DisableConsoleColor()

	// 创建带默认 middleware 的路由:
	// logger and recovery (crash-free) middleware
	router := gin.Default()

	router.GET("/someGet", getting)
	router.POST("/somePost", posting)
	router.PUT("/somePut", putting)
	router.DELETE("/someDelete", deleting)
	router.PATCH("/somePatch", patching)
	router.HEAD("/someHead", head)
	router.OPTIONS("/someOptions", options)

	// 默认监听 :8080 端口 除非
	// 在参数中中传入自定义端口.
	router.Run()
	// router.Run(":3000") for a hard coded port
}
```

### 路由 path 里里的参数

```go
func main() {
	router := gin.Default()

	// 该 handle 会匹配 /user/john 这样的请求但不会匹配 /user/ 或 /user这样的请求
	router.GET("/user/:name", func(c *gin.Context) {
		name := c.Param("name")
		c.String(http.StatusOK, "Hello %s", name)
	})

	// 但像下面这样的路由，无论是 /user/john/ 还是 /user/john/send都会被匹配到
	// 如果没有其他路由匹配 /user/john, 那下面的代码将会重定向到  /user/john/
	router.GET("/user/:name/*action", func(c *gin.Context) {
		name := c.Param("name")
		action := c.Param("action")
		message := name + " is " + action
		c.String(http.StatusOK, message)
	})

	router.Run(":8080")
}
```

### 解析 URL 里的参数

```go
func main() {
	router := gin.Default()

	// 使用底层的请求对象解析URL里的查询参数
	// 此段代码处理类似这样的请求：  /welcome?firstname=Jane&lastname=Doe
	router.GET("/welcome", func(c *gin.Context) {
		firstname := c.DefaultQuery("firstname", "Guest")
		lastname := c.Query("lastname") // shortcut for c.Request.URL.Query().Get("lastname")

		c.String(http.StatusOK, "Hello %s %s", firstname, lastname)
	})
	router.Run(":8080")
}
```

### Multipart/Urlencoded Form

```go
func main() {
	router := gin.Default()

	router.POST("/form_post", func(c *gin.Context) {
		message := c.PostForm("message")
		nick := c.DefaultPostForm("nick", "anonymous")

		c.JSON(200, gin.H{
			"status":  "posted",
			"message": message,
			"nick":    nick,
		})
	})
	router.Run(":8080")
}
```

### 同时解析URL参数和表单POST参数的例子

```
POST /post?id=1234&page=1 HTTP/1.1
Content-Type: application/x-www-form-urlencoded

name=manu&message=this_is_great
```

```go
func main() {
	router := gin.Default()

	router.POST("/post", func(c *gin.Context) {

		id := c.Query("id")
		page := c.DefaultQuery("page", "0")
		name := c.PostForm("name")
		message := c.PostForm("message")

		fmt.Printf("id: %s; page: %s; name: %s; message: %s", id, page, name, message)
	})
	router.Run(":8080")
}
```

```
id: 1234; page: 1; name: manu; message: this_is_great
```

### 解析键值对或post表单里的参数

```
POST /post?ids[a]=1234&ids[b]=hello HTTP/1.1
Content-Type: application/x-www-form-urlencoded

names[first]=thinkerou&names[second]=tianou
```

```go
func main() {
	router := gin.Default()

	router.POST("/post", func(c *gin.Context) {

		ids := c.QueryMap("ids")
		names := c.PostFormMap("names")

		fmt.Printf("ids: %v; names: %v", ids, names)
	})
	router.Run(":8080")
}
```

```
ids: map[b:hello a:1234], names: map[second:tianou first:thinkerou]
```

### 处理文件上传

#### 单文件上传

References issue [#774](https://github.com/gin-gonic/gin/issues/774) and detail [example code](examples/upload-file/single).

```go
func main() {
	router := gin.Default()
	// 设置更小的上传文件大小的阈值 (默认可上传文件大小阈值为 32 MiB)
	// router.MaxMultipartMemory = 8 << 20  // 8 MiB
	router.POST("/upload", func(c *gin.Context) {
		// single file
		file, _ := c.FormFile("file")
		log.Println(file.Filename)

		// 保存文件到指定位置
		// c.SaveUploadedFile(file, dst)

		c.String(http.StatusOK, fmt.Sprintf("'%s' uploaded!", file.Filename))
	})
	router.Run(":8080")
}
```

How to `curl`:

```bash
curl -X POST http://localhost:8080/upload \
  -F "file=@/Users/appleboy/test.zip" \
  -H "Content-Type: multipart/form-data"
```

#### 多文件上传

See the detail [example code](examples/upload-file/multiple).

```go
func main() {
	router := gin.Default()
  // 设置更小的上传文件大小的阈值 (默认可上传文件大小阈值为 32 MiB)
	// router.MaxMultipartMemory = 8 << 20  // 8 MiB
	router.POST("/upload", func(c *gin.Context) {
		// 复合表单
		form, _ := c.MultipartForm()
		files := form.File["upload[]"]

		for _, file := range files {
			log.Println(file.Filename)

			// 保存文件到指定位置
			// c.SaveUploadedFile(file, dst)
		}
		c.String(http.StatusOK, fmt.Sprintf("%d files uploaded!", len(files)))
	})
	router.Run(":8080")
}
```

How to `curl`:

```bash
curl -X POST http://localhost:8080/upload \
  -F "upload[]=@/Users/appleboy/test1.zip" \
  -F "upload[]=@/Users/appleboy/test2.zip" \
  -H "Content-Type: multipart/form-data"
```

### 路由分组

```go
func main() {
	router := gin.Default()

	// Simple group: v1
	v1 := router.Group("/v1")
	{
		v1.POST("/login", loginEndpoint)
		v1.POST("/submit", submitEndpoint)
		v1.POST("/read", readEndpoint)
	}

	// Simple group: v2
	v2 := router.Group("/v2")
	{
		v2.POST("/login", loginEndpoint)
		v2.POST("/submit", submitEndpoint)
		v2.POST("/read", readEndpoint)
	}

	router.Run(":8080")
}
```

### 使用不带默认 middleware 的路由

用

```go
r := gin.New()
```

取代：

```go
// Gin 默认路由使用了 Logger 和 Recovery middleware 
r := gin.Default()
```


### 使用 middleware
```go
func main() {
	// Creates a router without any middleware by default
	r := gin.New()

	// Global middleware
	// Logger middleware 会使用 gin.DefaultWriter 作为默认的日志输出中间件，即使设置了 GIN_MODE=release.
	// 默认的 gin.DefaultWriter = os.Stdout
	r.Use(gin.Logger())

	// 如果发生了 panics, recovery middleware 会输出 500 并且从panic 中恢复
	r.Use(gin.Recovery())

	// 你可以添加任意多的 middleware 
	r.GET("/benchmark", MyBenchLogger(), benchEndpoint)

	// 权限控制 group
	// authorized := r.Group("/", AuthRequired())
	// 等价于:
	authorized := r.Group("/")
	// 分组路由里的 middleware!
	// AuthRequired() middleware just in the "authorized" group.
	authorized.Use(AuthRequired())
	{
		authorized.POST("/login", loginEndpoint)
		authorized.POST("/submit", submitEndpoint)
		authorized.POST("/read", readEndpoint)

		// 嵌套的分组路由
		testing := authorized.Group("testing")
		testing.GET("/analytics", analyticsEndpoint)
	}

	// Listen and serve on 0.0.0.0:8080
	r.Run(":8080")
}
```

### 如何输出日志文件
```go
func main() {
    // 仅用带颜色的控制台输出，因为这里把日志写到文件里
    gin.DisableConsoleColor()

    // Logging to a file.
    f, _ := os.Create("gin.log")
    gin.DefaultWriter = io.MultiWriter(f)

    // 如果你想把日志同时输出到控制台和日志文件，可以这么写：
    // gin.DefaultWriter = io.MultiWriter(f, os.Stdout)

    router := gin.Default()
    router.GET("/ping", func(c *gin.Context) {
        c.String(200, "pong")
    })

    router.Run(":8080")
}
```

### Model 绑定和校验

将请求体中的数据绑定到 Go 的类型变量里，我们可以使用 model 绑定。Gin 支持 JSON, XML 和标准表单值的绑定 (foo=bar&boo=baz).

Gin 使用 [**go-playground/validator.v8**](https://github.com/go-playground/validator) 来校验. 看 [here](http://godoc.org/gopkg.in/go-playground/validator.v8#hdr-Baked_In_Validators_and_Tags) 查看完整的 tags 使用方法.

注意，在定义接收请求的 struct 类型定义中，需要在你想绑定的字段上设置对应的 tag。例如，当你想绑定 JSON 格式类型的请求体中的数据时，这样写： `json:"fieldname"`.

同时， Gin 提供了两种绑定的方法:
- **Type** - Must bind
  - **Methods** - `Bind`, `BindJSON`, `BindXML`, `BindQuery`
  - **Behavior** - These methods use `MustBindWith` under the hood. If there is a binding error, the request is aborted with `c.AbortWithError(400, err).SetType(ErrorTypeBind)`. This sets the response status code to 400 and the `Content-Type` header is set to `text/plain; charset=utf-8`. Note that if you try to set the response code after this, it will result in a warning `[GIN-debug] [WARNING] Headers were already written. Wanted to override status code 400 with 422`. If you wish to have greater control over the behavior, consider using the `ShouldBind` equivalent method.
- **Type** - Should bind
  - **Methods** - `ShouldBind`, `ShouldBindJSON`, `ShouldBindXML`, `ShouldBindQuery`
  - **Behavior** - These methods use `ShouldBindWith` under the hood. If there is a binding error, the error is returned and it is the developer's responsibility to handle the request and error appropriately.

When using the Bind-method, Gin tries to infer the binder depending on the Content-Type header. If you are sure what you are binding, you can use `MustBindWith` or `ShouldBindWith`.

You can also specify that specific fields are required. If a field is decorated with `binding:"required"` and has a empty value when binding, an error will be returned.

```go
// Binding from JSON
type Login struct {
	User     string `form:"user" json:"user" xml:"user"  binding:"required"`
	Password string `form:"password" json:"password" xml:"password" binding:"required"`
}

func main() {
	router := gin.Default()

	// Example for binding JSON ({"user": "manu", "password": "123"})
	router.POST("/loginJSON", func(c *gin.Context) {
		var json Login
		if err := c.ShouldBindJSON(&json); err != nil {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
			return
		}
		
		if json.User != "manu" || json.Password != "123" {
			c.JSON(http.StatusUnauthorized, gin.H{"status": "unauthorized"})
			return
		} 
		
		c.JSON(http.StatusOK, gin.H{"status": "you are logged in"})
	})

	// Example for binding XML (
	//	<?xml version="1.0" encoding="UTF-8"?>
	//	<root>
	//		<user>user</user>
	//		<password>123</user>
	//	</root>)
	router.POST("/loginXML", func(c *gin.Context) {
		var xml Login
		if err := c.ShouldBindXML(&xml); err != nil {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
			return
		}
		
		if xml.User != "manu" || xml.Password != "123" {
			c.JSON(http.StatusUnauthorized, gin.H{"status": "unauthorized"})
			return
		} 
		
		c.JSON(http.StatusOK, gin.H{"status": "you are logged in"})
	})

	// Example for binding a HTML form (user=manu&password=123)
	router.POST("/loginForm", func(c *gin.Context) {
		var form Login
		// This will infer what binder to use depending on the content-type header.
		if err := c.ShouldBind(&form); err != nil {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
			return
		}
		
		if form.User != "manu" || form.Password != "123" {
			c.JSON(http.StatusUnauthorized, gin.H{"status": "unauthorized"})
			return
		} 
		
		c.JSON(http.StatusOK, gin.H{"status": "you are logged in"})
	})

	// Listen and serve on 0.0.0.0:8080
	router.Run(":8080")
}
```

**Sample request**
```shell
$ curl -v -X POST \
  http://localhost:8080/loginJSON \
  -H 'content-type: application/json' \
  -d '{ "user": "manu" }'
> POST /loginJSON HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.51.0
> Accept: */*
> content-type: application/json
> Content-Length: 18
>
* upload completely sent off: 18 out of 18 bytes
< HTTP/1.1 400 Bad Request
< Content-Type: application/json; charset=utf-8
< Date: Fri, 04 Aug 2017 03:51:31 GMT
< Content-Length: 100
<
{"error":"Key: 'Login.Password' Error:Field validation for 'Password' failed on the 'required' tag"}
```

**Skip validate**

When running the above example using the above the `curl` command, it returns error. Because the example use `binding:"required"` for `Password`. If use `binding:"-"` for `Password`, then it will not return error when running the above example again.

### Custom Validators

It is also possible to register custom validators. See the [example code](examples/custom-validation/server.go).

[embedmd]:# (examples/custom-validation/server.go go)
```go
package main

import (
	"net/http"
	"reflect"
	"time"

	"github.com/gin-gonic/gin"
	"github.com/gin-gonic/gin/binding"
	"gopkg.in/go-playground/validator.v8"
)

// Booking contains binded and validated data.
type Booking struct {
	CheckIn  time.Time `form:"check_in" binding:"required,bookabledate" time_format:"2006-01-02"`
	CheckOut time.Time `form:"check_out" binding:"required,gtfield=CheckIn" time_format:"2006-01-02"`
}

func bookableDate(
	v *validator.Validate, topStruct reflect.Value, currentStructOrField reflect.Value,
	field reflect.Value, fieldType reflect.Type, fieldKind reflect.Kind, param string,
) bool {
	if date, ok := field.Interface().(time.Time); ok {
		today := time.Now()
		if today.Year() > date.Year() || today.YearDay() > date.YearDay() {
			return false
		}
	}
	return true
}

func main() {
	route := gin.Default()

	if v, ok := binding.Validator.Engine().(*validator.Validate); ok {
		v.RegisterValidation("bookabledate", bookableDate)
	}

	route.GET("/bookable", getBookable)
	route.Run(":8085")
}

func getBookable(c *gin.Context) {
	var b Booking
	if err := c.ShouldBindWith(&b, binding.Query); err == nil {
		c.JSON(http.StatusOK, gin.H{"message": "Booking dates are valid!"})
	} else {
		c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
	}
}
```

```console
$ curl "localhost:8085/bookable?check_in=2018-04-16&check_out=2018-04-17"
{"message":"Booking dates are valid!"}

$ curl "localhost:8085/bookable?check_in=2018-03-08&check_out=2018-03-09"
{"error":"Key: 'Booking.CheckIn' Error:Field validation for 'CheckIn' failed on the 'bookabledate' tag"}
```

[Struct level validations](https://github.com/go-playground/validator/releases/tag/v8.7) can also be registered this way.
See the [struct-lvl-validation example](examples/struct-lvl-validations) to learn more.

### Only Bind Query String

`ShouldBindQuery` function only binds the query params and not the post data. See the [detail information](https://github.com/gin-gonic/gin/issues/742#issuecomment-315953017).

```go
package main

import (
	"log"

	"github.com/gin-gonic/gin"
)

type Person struct {
	Name    string `form:"name"`
	Address string `form:"address"`
}

func main() {
	route := gin.Default()
	route.Any("/testing", startPage)
	route.Run(":8085")
}

func startPage(c *gin.Context) {
	var person Person
	if c.ShouldBindQuery(&person) == nil {
		log.Println("====== Only Bind By Query String ======")
		log.Println(person.Name)
		log.Println(person.Address)
	}
	c.String(200, "Success")
}

```

### Bind Query String or Post Data

See the [detail information](https://github.com/gin-gonic/gin/issues/742#issuecomment-264681292).

```go
package main

import (
	"log"
	"time"

	"github.com/gin-gonic/gin"
)

type Person struct {
	Name     string    `form:"name"`
	Address  string    `form:"address"`
	Birthday time.Time `form:"birthday" time_format:"2006-01-02" time_utc:"1"`
}

func main() {
	route := gin.Default()
	route.GET("/testing", startPage)
	route.Run(":8085")
}

func startPage(c *gin.Context) {
	var person Person
	// If `GET`, only `Form` binding engine (`query`) used.
	// If `POST`, first checks the `content-type` for `JSON` or `XML`, then uses `Form` (`form-data`).
	// See more at https://github.com/gin-gonic/gin/blob/master/binding/binding.go#L48
	if c.ShouldBind(&person) == nil {
		log.Println(person.Name)
		log.Println(person.Address)
		log.Println(person.Birthday)
	}

	c.String(200, "Success")
}
```

Test it with:
```sh
$ curl -X GET "localhost:8085/testing?name=appleboy&address=xyz&birthday=1992-03-15"
```

### Bind HTML checkboxes

See the [detail information](https://github.com/gin-gonic/gin/issues/129#issuecomment-124260092)

main.go

```go
...

type myForm struct {
    Colors []string `form:"colors[]"`
}

...

func formHandler(c *gin.Context) {
    var fakeForm myForm
    c.ShouldBind(&fakeForm)
    c.JSON(200, gin.H{"color": fakeForm.Colors})
}

...

```

form.html

```html
<form action="/" method="POST">
    <p>Check some colors</p>
    <label for="red">Red</label>
    <input type="checkbox" name="colors[]" value="red" id="red" />
    <label for="green">Green</label>
    <input type="checkbox" name="colors[]" value="green" id="green" />
    <label for="blue">Blue</label>
    <input type="checkbox" name="colors[]" value="blue" id="blue" />
    <input type="submit" />
</form>
```

result:

```
{"color":["red","green","blue"]}
```