## http概念
它是一种客户端和服务器之间通信的协议。
最常见的场景是：
```go
浏览器 / App / 前端页面  --->  服务器
        请求 Request

浏览器 / App / 前端页面  <---  服务器
        响应 Response
```
## http请求Request
一个 HTTP 请求通常由几部分组成：
```
请求方法 URL 协议版本
请求头 Headers

请求体 Body
```
例如：
```
POST /users HTTP/1.1
Host: example.com
Content-Type: application/json
Authorization: Bearer token123

{
  "name": "Tom",
  "age": 18
}
```
### 1.请求方法Method
HTTP 请求方法表示客户端想对服务器资源做什么操作。
常见方法有：

|方法|含义|常见用途|
|---|---|---|
|GET|获取资源|查询用户、获取列表|
|POST|创建资源|新增用户、提交表单|
|PUT|整体更新资源|修改用户完整信息|
|PATCH|部分更新资源|修改用户名、状态|
|DELETE|删除资源|删除用户|
|OPTIONS|预检请求|跨域场景常见|
例如：

```
GET /users
```

表示获取用户列表。

```
POST /users
```

表示创建一个用户。

在 Gin 中会这样写：

```go
r.GET("/users", func(c *gin.Context) {
    c.JSON(200, gin.H{"message": "获取用户列表"})
})

r.POST("/users", func(c *gin.Context) {
    c.JSON(200, gin.H{"message": "创建用户"})
})
```
## URL 和路径
HTTP 请求中有一个非常重要的部分：URL。
例如：
```
https://api.example.com/users/100?keyword=tom&page=1
```
可以拆成：
```
协议：https
域名：api.example.com
路径：/users/100
查询参数：keyword=tom&page=1
```
### 1. 路径参数

```
/users/100
```

这里的 `100` 通常表示用户 ID。

Gin 中可以这样接收：

```go
r.GET("/users/:id", func(c *gin.Context) {
    id := c.Param("id")
    c.JSON(200, gin.H{"id": id})
})
```

访问：

```
GET /users/100
```

返回：

```
{  "id": "100"}
```

### 2. 查询参数

```
/users?keyword=tom&page=1
```

`keyword` 和 `page` 就是查询参数。

Gin 中可以这样接收：

```go
r.GET("/users", func(c *gin.Context) {
    keyword := c.Query("keyword")
    page := c.DefaultQuery("page", "1")

    c.JSON(200, gin.H{
        "keyword": keyword,
        "page": page,
    })
})
```
## HTTP 请求头 Headers
请求头用于传递一些额外信息.
例如：
```
Content-Type: application/json
Authorization: Bearer token123
User-Agent: Mozilla/5.0
```
常见请求头：

| 请求头           | 作用           |
| ------------- | ------------ |
| Content-Type  | 请求体的数据格式     |
| Authorization | 身份认证信息       |
| User-Agent    | 客户端信息        |
| Cookie        | 携带 Cookie    |
| Accept        | 客户端希望接收的数据类型 |
## HTTP 请求体 Body
请求体一般用于提交数据，常见于 POST、PUT、PATCH 请求。

## HTTP 响应 Response
服务器处理请求后，会返回 HTTP 响应。

响应通常包括：
```
协议版本 状态码 状态描述
响应头 Headers

响应体 Body
```
例如：
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "success"
}
```
## HTTP 状态码
状态码用于表示请求处理结果。

常见状态码：

| 状态码 | 含义                    | 场景       |
| --- | --------------------- | -------- |
| 200 | OK                    | 请求成功     |
| 201 | Created               | 创建成功     |
| 400 | Bad Request           | 请求参数错误   |
| 401 | Unauthorized          | 未登录或认证失败 |
| 403 | Forbidden             | 没有权限     |
| 404 | Not Found             | 资源不存在    |
| 500 | Internal Server Error | 服务器内部错误  |
## 总结
```
1. 客户端发送 Request
2. 服务端返回 Response
3. Request 里有 Method、URL、Headers、Body
4. Response 里有 Status Code、Headers、Body
5. GET 常用于查询
6. POST 常用于提交或创建
7. URL 里可以有路径参数和查询参数
8. Body 里可以提交 JSON
9. HTTP 本身是无状态的
10. Gin 的核心工作就是处理 HTTP 请求并返回 HTTP 响应
```