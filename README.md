# 普通用户

```sql
CREATE TABLE `scuec`.`user_info` (
  `username` VARCHAR(255) NOT NULL,
  `password` VARCHAR(255) NOT NULL,
  `college` VARCHAR(255) NOT NULL,
  `type` INT NOT NULL,
  `name` VARCHAR(255) NOT NULL,
  `phone_number` VARCHAR(255) NOT NULL,
  PRIMARY KEY (`username`),
  INDEX idx_phone_number (phone_number)
)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8
COLLATE = utf8_bin;
```

```sql
CREATE TABLE `scuec`.`info_20211234` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `minus` INT NOT NULL, -- 今日离校
  `addition` INT NOT NULL, -- 今日返校
  `sum` INT NOT NULL, -- 今日在校
  `collegesum` INT NOT NULL, -- 学院总人数
  `college` VARCHAR(255) NOT NULL, -- 学院名
  `type` INT NOT NULL, -- 本/研
  `head` VARCHAR(255) NOT NULL, -- 负责人账号
  `head_name` VARCHAR(255) NOT NULL, -- 负责人姓名
  `head_phone` VARCHAR(255) NOT NULL, -- 负责人电话
  `remarks` VARCHAR(255) NOT NULL, -- 备注
  `date` VARCHAR(255) NOT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8
COLLATE = utf8_bin;
```

```sql
CREATE TABLE `scuec`.`history` (
    id INT AUTO_INCREMENT,
    sumben INT NOT NULL,
    minusben INT NOT NULL,
    additionben INT NOT NULL,
    sumyan INT NOT NULL,
    minusyan INT NOT NULL,
    additionyan INT NOT NULL,
    date VARCHAR(255) NOT NULL,
    PRIMARY KEY (`id`)
)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8
COLLATE = utf8_bin;
```



## 登录

/login

### 账号密码登录

post请求

```json
{
    "username":"",
    "password":""
}
```

返回值：

```json
{
    "code":200,
    "msg":"",
    data:{
            "code": 200,
    "msg": "登录成功",
    "data": {
        "username": "20211234",
        "college": "计算机科学学院",
        "type": 1,
        "name": "阿禾.朱力黑沙",
        "phone_number": "13366667777",
        "power": false,
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6IjIwMjExMjM0IiwicG93ZXIiOmZhbHNlLCJzdWIiOiJBaXJjb25kaXRpb25TeXN0ZW1KV1QiLCJleHAiOjE2OTY1ODQ5NjMsImp0aSI6IjgwYTVmYTRmLTg1ZTEtNDJiZC05ZjgxLWFmYTY4YzgzZGU4MiJ9.KjJF-D8LpK5HanMLAp_VMjl8PMlF3ACMoXbUHQH4WCI"
    	}
    }
}
```

code 200代表登录成功 201代表登录失败

登录成功后会返回

### 验证token有效

get请求

在请求头中带上

token:"xxxxxxxxx"

返回值：

```json
data{
    code: 200
    data: {
        college: "民族学与社会学学院"
        name: "马博博"
        openId: "o65Z268i7G1_rVQozlMmti2Gx2GM"
        phone_number: "17713578195"
        power: false
        token: null
        type: 1
        username: "9446582"
    }
    msg: "token有效"
}
```

code 200代表有效 201代表无效

## 填写（旧版）

传入今日在校人数(sum)、今日离校人数(minus)、今日返校人数(addition)

## 填写（新版）

传入今日在校人数(sum)、今日离校人数(minus)、今日返校人数(addition)、学院总人数（collegesum）、备注（remarks）

## 修改密码接口

/alldata

post请求

```json
传参：请求头带token：键用"token"，值用账号密码登录成功时返回的token值
请求体：

```



## 判断是否成功





# 管理员

## 登录接口

```
账号密码登录+微信登录
账号密码登录后可以获得一个token值存入；微信登录可以获得openid和session_key（后续订阅需要用到）
注意：session_key是会过期的，所以再每次使用时要先检测是否过期（wx.checkSession判断是否过期，如果过期再次调用登录方法，更新session_key）
```

#### 账号密码登录

/admin

post请求

```json
{
	"username":"",
    "password":""
}
```

返回值

```json
//非管理员，权限不足
{
    "code": 205,
    "msg": "权限不足",
    "data": null
}

//账号或密码错误
{
    "code": 201,
    "msg": "用户名或密码错误",
    "data": null
}

//登录成功
{
    "code": 200,
    "msg": "登录成功",
    "data": {
        "username": "123456",
        "college": "学工",
        "type": 1,
        "name": "管理员",
        "phone_number": "15588889999",
        "power": true,
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6IjEyMzQ1NiIsInBvd2VyIjp0cnVlLCJzdWIiOiJBaXJjb25kaXRpb25TeXN0ZW1KV1QiLCJleHAiOjE2OTY5MDUyNTEsImp0aSI6IjRkYWIwZmE1LWFlOTMtNGZjMS04MzhhLTJjY2EzZWViNmM2NCJ9.0ill_EstulDShceYJOvUvDeZ3kkMlZjm2Am2tCmzrn4"
    }
}
```



#### 微信登录

/wxlogin

post请求

```json
{
    "code":""
}
```

返回值

```json
{
    "code": 200,
    "msg": "success",
    "data": {
        "openid": "o65Z26-yN8zFfGmkC9KxTb9clg94",
        "session_key": "KLp/bYb1asiikrVCYQmdCg=="
    }
}
//注意：session_key是会过期的，所以再每次使用时要先检测是否过期（wx.checkSession判断是否过期，如果过期再次调用登录方法，更新session_key）
```



## 订阅消息接口

/subscribe

post请求

```json
请求头带token  (键为token，值为登录成功时返回的token)
请求体：
{
	"openid":"" //微信登录成功时返回的openid
}
```



## 数据总览接口

/alldata

get请求

```
数据总览，获取所有学院填写的最新信息，进行相加得到结果返回
```

```
传参：请求头带token：键用"token"，值用账号密码登录成功时返回的token值
```

```json
返回值：
{
    "code": 200,
    "msg": "获取成功",
    "data": {
        "college": { //本科生
            "minus": 20, //离校人数
            "sum": 550, //在校人数
            "addition": 70 //返校人数
        },
        "all": {
            "minus": 23,
            "sum": 557,
            "addition": 80
        },
        "master": {
            "minus": 3,
            "sum": 7,
            "addition": 10
        }
    }
}
```



## 下载文件接口

/download

get请求

无需任何参数或请求头

### 下载本科生各个学院

/download/college

get请求

无需任何参数或请求头

### 下载研究生各个学院

/download/master

get请求

无需任何参数或请求头



## 获取本科生数据接口

/college

### 已提交的学院

post请求

```
传参：请求头带token：键用"token"，值用账号密码登录成功时返回的token值
无需带任何请求体参数
```

```json
返回值：
{
    "code": 200,
    "msg": "获取成功",
    "data": [
        {
            "college": "计算机科学学院", //学院名
            "minus": 20, //离校人数
            "sum": 550, //在校人数
            "addition": 70 //返校人数
        },
        {
            "college": "教育学院",
            "minus": 44,
            "sum": 55,
            "addition": 33
        }
    ]
}
```



### 未提交的学院

put请求



## 获取研究生数据接口

/master

### 已提交的学院

post请求

```
传参：请求头带token：键用"token"，值用账号密码登录成功时返回的token值
无需带任何请求体参数
```

```json
返回值：
{
    "code": 200,
    "msg": "获取成功",
    "data": [
        {
            "college": "教育学院",
            "minus": 44,
            "sum": 55,
            "addition": 33
        }
    ]
}
```



### 未提交的学院

put请求

```
传参：请求头带token：键用"token"，值用账号密码登录成功时返回的token值
无需带任何请求体参数
```

```json
返回值：
{
    "code": 200,
    "msg": "获取成功",
    "data": [
        "马克思主义学院"
    ]
}
```



## 增删账号接口

/usermanager

#### 添加

post请求

```json
传参：请求头带token：键用"token"，值用账号密码登录成功时返回的token值
请求体：
{
    "password":"123",//密码
    "college":"",//学院名称
    "type":1,//类型 1代表本科生 2代表研究生（这里前端建议给选择框） 
    "name":"",//姓名
    "phone_number":""//电话号码
}
```

```json
返回值：
{
    "code": 200, //200代表添加成功 201代表添加失败
    "msg": "添加成功",
    "data": "8245551" //账号
}
```



#### 删除

delete请求

```json
传参：请求头带token：键用"token"，值用账号密码登录成功时返回的token值
请求体：
{
    "username":"9855506"
}
```

```json
返回值：
{
    "code": 200,
    "msg": "删除成功",
    "data": null
}
```

## 历史记录

### 昨天

/one

#### 下载数据

get请求



#### 查询

post请求

```
传参：请求头带token：键用"token"，值用账号密码登录成功时返回的token值
无需带任何请求体参数
```

```json
返回值：
{
    "code": 200,
    "msg": "查询成功",
    "data": {
        "sumben": 24347,
        "minusben": 27,
        "additionben": 36,
        "otherben": 883,
        "collegesumben": 25428,
        "sumyan": 3917,
        "minusyan": 8,
        "additionyan": 6,
        "otheryan": 353,
        "collegesumyan": 4422,
        "sum": 28264,
        "minus": 35,
        "addition": 42,
        "other": 1236,
        "collegesum": 29850,
        "date": "2023-10-12"
    }
}
```



### 前天

/two

#### 下载数据

get请求



#### 查询

post请求

```
传参：请求头带token：键用"token"，值用账号密码登录成功时返回的token值
无需带任何请求体参数
```

```json
返回值：
{
    "code": 200,
    "msg": "查询成功",
    "data": {
        "sumben": 24347,
        "minusben": 27,
        "additionben": 36,
        "otherben": 883,
        "collegesumben": 25428,
        "sumyan": 3917,
        "minusyan": 8,
        "additionyan": 6,
        "otheryan": 353,
        "collegesumyan": 4422,
        "sum": 28264,
        "minus": 35,
        "addition": 42,
        "other": 1236,
        "collegesum": 29850,
        "date": "2023-10-12"
    }
}
```



### 前两天

/three

#### 下载数据

get请求

#### 查询

post请求

```
传参：请求头带token：键用"token"，值用账号密码登录成功时返回的token值
无需带任何请求体参数
```

```json
返回值：
{
    "code": 200,
    "msg": "查询成功",
    "data": {
        "sumben": 24347,
        "minusben": 27,
        "additionben": 36,
        "otherben": 883,
        "collegesumben": 25428,
        "sumyan": 3917,
        "minusyan": 8,
        "additionyan": 6,
        "otheryan": 353,
        "collegesumyan": 4422,
        "sum": 28264,
        "minus": 35,
        "addition": 42,
        "other": 1236,
        "collegesum": 29850,
        "date": "2023-10-12"
    }
}
```



