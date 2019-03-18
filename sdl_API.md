### 设计与需求阶段

#### 身份认证

原则:使用成熟且安全的组件 不要造轮子

* 使用标准的认证协议(如 JWT, OAuth). 不要使用Basic Auth
* 使用标准的 Authentication, token generating, password storing
* 在登录中使用 Max Retry 和自动封禁功能
* 加密所有的敏感数据

#### JWT (JSON Web Token)

* 使用随机复杂的密钥(`JWT Secret`) 以增加暴力枚举的难度
* 后端强制使用高强度算法(`HS256` or `RS256`). 不要从请求的payload中直接提取数据
* 设置尽可能短的token过期时间 token expiration (`TTL`, `RTTL`)
* 不要在JWT的payload中存储敏感数据 因为 [很容易被解码](https://jwt.io/#debugger-io)

#### OAuth

* 后端需要验证`redirect_uri` 且只允许白名单中的URL
* 始终尝试交换code而不是交换token (不允许 `response_type=token`)
* 使用 `state` 参数并填充随机的hash 以防御OAuth authentication过程中的CSRF(跨站请求伪造)
* 对不同的应用定义各自的 `default scope`参数(默认的作用区域) 和 `validate scope`参数(有效的作用区域)


#### Access

* 限制requests(Throttling) 以避免DDoS和暴力枚举攻击
* 使用HTTPS 以避免MITM (Man in the Middle Attack)
* 使用`HSTS` 以避免SSL Strip攻击


#### Input

* 根据操作 使用恰当的HTTP方法 `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)`,`DELETE (to delete a record)` 如果请求的方法不适用于请求的资源则返回 `405 Method Not Allowed`
* 验证`content-type` 只允许后端支持的格式 (如`application/xml`, `application/json`)  并在不满足条件的时候返回 `406 Not Acceptable`
