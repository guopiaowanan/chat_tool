# 基于C/S模式的通信工具

### 通信协议

1. 基于TCP协议，服务器默认端口号为9999；

   

2. 登录校验：

   ​	客户端连接服务器成功后，客户端将用户名和密码信息（15字节定长包头 + JSON数据）发送给服务器端，服务器端校验用户名和密码，将校验结果发送给客户端，响应数据（15字节定长包头 + JSON数据）

   

3. 注册功能：

   ​	客户端连接服务器成功后，客户端将用户注册信息（15字节定长包头 + JSON数据）发送给服务器端，服务器端校验用户注册信息，就将用户注册信息保存到数据库，并给客户端发送一个响应数据（15字节定长包头 + JSON数据），如果失败就直接给客户端发送一个响应数据（15字节定长包头 + JSON数据）。

   

4. 聊天信息：（15字节定长包头 + JSON数据） 

5. 不同情况下的JSON数据格式定义如下：

   **用户登录请求（邮箱）：**

   {	

   ​	"op": 11,

   ​	"args": 	{	"uLogin": "xxx@xxx.com",

   ​				"password": "A24314FBECDFDASFD4143241",   # 真实密码的MD5值，使用大写表示

   ​		 	  }

   }

   **用户登录请求（手机号）：**

   {	

   ​	"op": 12,

   ​	"args":	{	"uLogin": "18574315251",

   ​				"password": "A24314FBECDFDASFD4143241",   # 真实密码的MD5值，使用大写表示

   ​			}

   }

   **用户登录响应：**

   {	"op": 1,

   ​	"error_code": 0  # 0表示登录成功，1表示登录失败

   }

   **用户注册请求：**

   {	

   ​	"op": 2,

   ​	"args": 	{"uname": "小魔仙",

   ​				"passwd": "A24314FBECDFDASFD4143241",   # 真实密码的MD5值，使用大写表示

   ​				"phone": "18574315251",  # 手机号

   ​				"email": "xxx@email.com"  # 邮箱

   ​			}

   }

   **用户注册响应：**

   {	"op": 2,

   ​	"error_code": 0  # 0表示注册成功，1表示注册失败

   }

   **用户聊天消息请求：**

   {	

   ​	"op": 31,

   ​	"args":	{	 "from": "phone",

                    "to": "group", 

   ​				  "content": "聊天内容...",   

   ​			} # 发送群聊消息

   }

   {	

   ​	"op": 32,

   ​	"args":	{	"from": "phone",

                   	 "to": "12345678911", 

   ​				  "content": "聊天内容...",   


   ​			}	# 发送私聊消息


   }

### 数据库设计

**功能模块**

- 注册功能
- 登录功能

- 群聊功能
- 私聊功能

用户信息表（csUser）

| 表字段        | 说明                   |
| :------------ | ---------------------- |
| uid           | 用户ID，主键，大于1000 |
| uname         | 用户名，可重复         |
| phone         | 手机号，不可重复       |
| upass         | 密码                   |
| email         | 邮箱，不可重复         |
| regTime       | 注册时间               |
| lastLoginTime | 最近登录时间           |
