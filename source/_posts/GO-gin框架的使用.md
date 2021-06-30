---
title: GO-gin框架的使用
date: 2021-06-29 17:53:38
tags: [GO, gin]
---
**GO gin框架的使用以及遇到的坑**

<!--more-->

# 安装gin
```sh
go get -u github.com/gin-gonic/gin
```
## 坑！！安装完成之后在项目中无法使用gin框架
报错如下
![](gin_err.png)

解决办法，进入到对应的文件夹，使用`go mod`添加引用库。
例如我的文件夹名字是server

```sh
cd server
go mod init server
go mod tidy # 添加引用
```

## 启动服务

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
	r.Run() 
	// 访问 127.0.0.1:8080/ping
}
```

![](get_ping.png)

# 获取参数
## 获取path上的参数
```go
package main

import (
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
	// GET请求
	r.GET("/user/:name", func(c *gin.Context) {
		name := c.Param("name")
		c.JSON(200, gin.H{
			"name": name,
		})
	})
	r.GET("/user/:name/*action", func(c *gin.Context) {
		name := c.Param("name")
		action := c.Param("action")
		message := name + " is " + action
		c.JSON(200, gin.H{
			"name":    name,
			"action":  action,
			"message": message,
		})
	})
	
	// POST 请求
	r.POST("/user/:name/*action", func(c *gin.Context) {
		ok := c.FullPath() == "/user/:name/*action" // true
		if ok {
			name := c.Param("name")
			action := c.Param("action")
			c.JSON(200, gin.H{
				"status": ok,
				"name":   name,
				"action": action,
			})
		}
	})
	r.Run()
}
```
![](user_name_get.png)
![](user_name_post.png)
*疑问：为什么用‘***’接收Path参数时，得到的是一个‘/action’*

## 获取parameters上的参数
```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
	r.GET("/welcome", func(c *gin.Context) {
	// 在没有传入firstname参数时，firstname默认为Guest
		firstname := c.DefaultQuery("firstname", "Guest")
		lastname := c.Query("lastname")
		c.String(http.StatusOK, "Hello %s %s", firstname, lastname)
	})
	r.Run()
}

```
![](welcome_one.png)
![](welcome_tow.png)

## 获取form_data参数
```go
package main

import (
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
	r.POST("/form_post", func(c *gin.Context) {
		message := c.PostForm("message")
		nick := c.DefaultPostForm("nick", "anonymous")

		c.JSON(200, gin.H{
			"status":  "posted",
			"message": message,
			"nick":    nick,
		})
	})
	r.Run()
}
```
![](form_data_post.png)

## post 获取URL上的参数
```go
package main

import (
	"fmt"

	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
	r.POST("/post", func(c *gin.Context) {
    // 取URL中的参数
		id := c.Query("id")
		// 如果URL中没有对应的值page 则默认值为0
		page := c.DefaultQuery("page", "0")
		// 从form_data中取参数
		name := c.PostForm("name")
		message := c.PostForm("message")

		fmt.Printf("id: %s; page: %s; name: %s; message: %s", id, page, name, message)
	})
	r.Run()
}
```
`127.0.0.1:8080/post?id=1234&page=1&message=哈哈哈哈&name=Jack`

```sh
id: 1234; page: 1; name: ; message: [GIN] 2021/06/29 - 22:19:27 | 200 |      45.136µs |       127.0.0.1 | POST     "/post?id=1234&page=1&message=%E5%93%88%E5%93%88%E5%93%88%E5%93%88&name=Jack"
```
## post获取map表单参数
```go
package main

import (
	"fmt"

	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
	r.POST("/post", func(c *gin.Context) {

		ids := c.QueryMap("ids")
		names := c.PostFormMap("names")

		fmt.Printf("ids: %v; names: %v", ids, names)
	})
	r.Run()
}

```

![](post_map.png)

```sh
ids: map[a:1234 b:hello]; names: map[a:哈哈哈 b:呵呵呵][GIN] 2021/06/29 - 22:43:02 | 200 |     498.003µs |       127.0.0.1 | POST     "/post?ids[a]=1234&ids[b]=hello"
```

## 分组路由-Grouping routes
```go
package main

import (
	"fmt"

	"github.com/gin-gonic/gin"
)

func main() {
	router := gin.Default()

	// Simple group: v1
	v1 := router.Group("/v1")
	{
		v1.POST("/login", loginEndpoint)
	}

	// Simple group: v2
	v2 := router.Group("/v2")
	{
		v2.POST("/login", login)
	}

	router.Run(":8080")

}

func loginEndpoint(c *gin.Context) {
	fmt.Println("v1 路由")
}

func login(c *gin.Context) {
	fmt.Println("v2 路由")
}

```

```sh
localhost:8080/v1/login
# v1 路由
localhost:8080/v2/login
# v2 路由
```

## 中间件
- 中间件可以预处理请求
- 如果请求中的参数不符合要求，那么将不会进入到路由函数中

```go
package main

import (
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.New()
	r.Use(MiddlewareName())
	r.GET("/setName", func(c *gin.Context) {
		// 如果满足中间件条件，则会进入此结构体中，此时可以做正确的返回操作
		name := c.Query("name")
		c.JSON(200, gin.H{
			"name": name,
		})
	})

}

func MiddlewareName() gin.HandlerFunc {
	return func(c *gin.Context) {
		name := c.Query("name")
		// 判断输入的字符是否小于5个
		if len(name) > 5 {
			c.AbortWithStatusJSON(400, "输入的年龄最大不能超过4个字符")
			return
		}
	}
}

```

```sh
localhost:8080/setname
# "name参数缺失"
localhost:8080/setname?name=Jack
# {
#    "name": "Jack"
# }
localhost:8080/setname?name=JackChen
# "输入的年龄最大不能超过4个字符"
```