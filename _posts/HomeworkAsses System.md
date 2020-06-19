# 作业自动评分系统总结报告
| 版本    | 修订人   | 修订日期   | 修订描述   | 
|:----|:----:|:----|:----|:----|
| V1.0   | 宋志超 石硕   | 2020-1-13   | 前端UI部分功能介绍   | 
| V2.0   | 许玉丹    | 2020-1-20   | 增加后端部分描述   | 
| V3.0   | 全体   | 2020-1-21   | 整体修订   | 

# **1.总体****概述**

## **1.1 ****功能摘要**

本系统为线上自动判分系统，分为登陆注册、教师管理、学生作答、后台管理四个模块，采用注册登录方式开放用户权限，以班级为主体，实现教师自主导入题目、发布作业、学生在线作答并自动判分的功能，此外还提供用户密码修改、成绩修改、学生成绩查询、违规行为检测处理、后台监控管理的功能，为师生提供一个作业交互的平台。

下面会详细介绍各个功能在UI中的布局，以及各自触发方式，和后端的数据交互形式等。

## **1.2 后端代码整体结构**

后端代码书写分为四个模块：注册登录、教师部分、学生部分、后台管理，每个模块路由分层管理（二或三层），数据库字段设置依据设计文档、结合具体实现过程进行了部分改进，每个模块功能接口采用http传输，token验证身份，后端接收解析前端传送的json数据，对接收请求进行处理，并根据接收数据对数据库进行操作，返回相应的http响应和前端需要的数据，除接收请求和响应请求基本操作外，每个模块详细功能及具体数据库操作会在下面介绍。

## **1. 3****模块功能流程****图****及实物展示图**

### **1.3****.1 登陆注册模块**

![图片](https://uploader.shimo.im/f/Gc5ruZF3LuUg5ijQ.png!thumbnail)

**实物图展示：**

![图片](https://uploader.shimo.im/f/ev9ZQdDHJewB1CMw.png!thumbnail)

图2.1 登录

### **1.3****.2 教师管理模块**

![图片](https://uploader.shimo.im/f/Ml3qqIYCRy4mkBci.png!thumbnail)

**教师模块实物展示：**

![图片](https://uploader.shimo.im/f/7UorJGHYGrk7HmCn.png!thumbnail)

图2.2.1 主页

![图片](https://uploader.shimo.im/f/Nq87XB4TCiQ9m5QF.png!thumbnail)

图2.2.2 信息中心

![图片](https://uploader.shimo.im/f/kQONJTeZKzwYZHFs.png!thumbnail)

图2.2.2 修改密码

### **1.3****.3 学生作答模块**

![图片](https://uploader.shimo.im/f/Huy2i5uRZ9UqDgON.png!thumbnail)

**学生界面的实物展示：（**号代表未跟数据库联通时候的界面）**

![图片](https://uploader.shimo.im/f/S1mNua5w3eImq5Xj.png!thumbnail)

### **1.3****.4 后台管理模块**

![图片](https://uploader.shimo.im/f/0oYv08uxmHEXaZy6.png!thumbnail)
管理员界面实物展示：  涉及到获取用户数据 搜索检索的功能 修改 开户 等![图片](https://uploader.shimo.im/f/lgJy4SwExOwZhZM7.png!thumbnail)


---
# **2.教师模块详细功能设计**

## 2.1 教师账户管理模块

### 2.1.1 功能描述

对老师的账户信息进行管理，包括登录，注册，修改密码，查找账户信息等

### **2.1.2 ****接口****实现**

### **2.1.2.1 ****教师登陆**

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----|:----:|:----|:----:|:----|:----:|:----|:----:|
| teacherCode | 教师工号   | String   | M   | Admin   | 
| password   | 密码   | String   | M   | 12345678   | 

**出参：**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----:|:----|:----:|:----|:----:|:----|:----:|:----|:----:|
| data   | 令牌   | String   | M   | a232e1b34412231ae   | 

**实物：**

![图片](https://uploader.shimo.im/f/ev9ZQdDHJewB1CMw.png!thumbnail)

**数据库操作**：检索教师表或学生表查找接收的学工号，核对表中密码与接收密码是否一致

**处理的报错情况**：查找用户不存在；输入密码不正确

### 2.1.2.2 账户注册

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| teacherCode   | 学工号   | varchar(50)   | M   | 教师工号   | 
| teacherName   | 姓名   | varchar(50)   | M   | 教师姓名   | 
| password   | 设置的密码   | varchar(50)   | M   | 口令的SHA-2指纹   | 
| phone   | 手机号   | varchar(50)   | O   | 找回密码用   | 
| email   | 邮箱   | varchar(50)   | O   | 找回密码用   | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**实物**：

![图片](https://uploader.shimo.im/f/HSXPKI4dcIodcLZx.png!thumbnail)

**后端数据库操作**：根据注册类型将注册信息写入教师表（类型号为1）或学生表（类型号为0），生成token并存储

**处理的报错情况**：注册用户已经存在

### 2.1.2.3 密码重置

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| teacherCode   | 学工号   | varchar(50)   | M   | 教师工号   | 
| password   | 新设置的密码   | varchar(50)   | M   | 口令的SHA-2指纹   | 
| phone   | 手机号   | varchar(50)   | M   | 找回密码用   | 
| email   | 邮箱   | varchar(50)   | M   | 找回密码用   | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**实物样例：**

![图片](https://uploader.shimo.im/f/Lik83ZvpxIs0XGOa.png!thumbnail)

## 2.2 教师题目管理模块

### 2.2.1 功能描述

教师进行，作业的上传，作业的发布，作业库信息的查看等

### **2.2.2 接口实现**

**2.2.2.0 主页初始化**

显示效果：

![图片](https://uploader.shimo.im/f/7UorJGHYGrk7HmCn.png!thumbnail)

**接收参数**：token

**数据库操作**：根据token解析出的用户学工号，对数据表检索返回该教师的姓名、学工号、已经发布的任务、其他教师发布的任务动态、所在班级人数信息

### 2.2.2.1 创建作业库

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| questionBankCode   | 作业库代码   | varchar(50)   | M   |    | 
| questionBankCreateby   | 创建人   | varchar(50)   | M   |    | 
| questionBankDesc   | 作业库描述   | varchar(255)   | O   |    | 
| questionPoints   | 作业库总分   | int(3)   | O   |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**实物样例：**

![图片](https://uploader.shimo.im/f/PelsPJsg8HsHkZdU.png!thumbnail)

![图片](https://uploader.shimo.im/f/DJvYYDNciJM44bnr.png!thumbnail)

**后端逻辑：**

①接收：token、作业库名称

②数据库操作：根据token解析出的用户学工号，在作业库数据表中写入新建的作业库名称、创建者和创建时间

③处理的报错信息：作业库名称已经存在

### 2.2.2.2 删除作业库

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| questionBankCode   | 作业库代码   | varchar(50)   | M   |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**示例：**

![图片](https://uploader.shimo.im/f/pr1MpMKaDlQJM5dR.png!thumbnail)

**后端逻辑：**

①接收：token、作业库名称

②数据库操作：根据token解析出的用户学工号，在作业库数据表中删除作业库名称、创建者和创建时间

### 2.2.2.3 录入题目

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| questionBankCode   | 作业库代码   | varchar(50)   | M   | 作业库代码   | 
| questionId   | 题目编号   | varchar(50)   | M   | 题目编号   | 
| questionType   | 题目类型   | varchar(50)   | M   | 题目类型   | 
| questionDesc   | 题干   | varchar(255)   | M   | 题干信息   | 
| questionDescType   | 题干导入类型   | tinyint(2)   | M   | 0表示文字导入，1表示图片导入   | 
| itemDetail   | 题目详细内容   | Map   | M   | "选择题：题目选项，正确选项，选项数量","填空题：正确答案","代码题目：标准输入，标准输出"   | 
| questionAnswType   | 题目答案导入类型   | tinyint(2)   | M   | 0表示文字导入，1表示图片导入   | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**示例：**

![图片](https://uploader.shimo.im/f/nSd1DHQEDxY85Zvu.png!thumbnail)

**接口描述：**

分三种不同类型的题目进行录入，题目录入后在后端根据不同类型的数据进行不同方式的存储，存储方式和调用方式更加清晰，此外图片类型通过base编码的方式进行传输，传输后保存在后端。

**后端逻辑：**

①接收：token、作业库名称、题目类型、题干、正确答案（选择填空）、题目分值、选项列表（选择）、标准输入输出和测试输入输出（编程）

②数据库操作：根据token解析出的用户学工号和接收的作业库名称，检验题目是否存在，不存在，在作业库详情数据表中写入题目信息，（选择：在选项数据表中写入选项信息，编程：在编程详情表中写入输入输出信息），已存在，更新题目信息；支持录入图片，接收的数据是base64编码后的图片数据，经解码后，将图片存储到特定文件夹，此时数据表中题干字段存储的是图片存放的地址

### 2.2.2.4 获取作业库列表

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| questionBankCode   | 作业库代码   | String   | O   | 过滤条件   | 
| startTime   | 搜索时间区间   | datetime   | O   | 筛选条件   | 
| deadTime   | 搜索时间区间   | datetime   | O   | 筛选条件   | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| data   | 作业库信息列表   | List of Object   |    |    | 
| ├─questionBankCode   | 作业库代码   | String   | M   | 过滤条件   | 
| ├─questionBankCreateby   | 创建人   | String   | M   | 创建人   | 
| ├─questionBankDesc   | 作业库描述   | String   | M   |    | 
| ├─createdAt   | 创建时间   | datetime   | M   |    | 
| ├─updatedAt   | 更新时间   | datetime   | M   |    | 

示例：

![图片](https://uploader.shimo.im/f/wgnN9MtjrCMu4Cfj.png!thumbnail)

**后端逻辑：**

①接收：token

②数据库操作：根据token解析出的用户学工号，根据学工号查询该学工号下，可以查看的任务列表，返回前端作为显示用。

### 2.2.2.5 发布任务

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| questionBankCode   | 作业库代码   | String   | M   |    | 
| taskDesc   | 任务描述   | String   | M   |    | 
| taskDeadline   | 任务截止时间   | datetime   | M   |    | 
| classCode   | 任务班级   | String   | M   |    | 
| taskCreateby   | 创建人   | String   | M   |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**示例：**

![图片](https://uploader.shimo.im/f/lmN9AE4emqwF3cbl.png!thumbnail)

**后端逻辑：**

①接收：token、任务名称、选择的作业库名称、发布的班级（可多个班级）、截止日期、任务描述

②数据库操作：根据token解析出的用户学工号，在任务列表中写入任务详情

③处理的错误情况：任务已存在

### 2.2.2.6 删除任务

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| taskName   | 任务名称   | varchar(50)   | M   |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**示例：**

![图片](https://uploader.shimo.im/f/4FJ9kEl59ZEnbxzd.png!thumbnail)

### 2.2.2.7 获得任务列表

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| taskName   | 任务名称   | String   | O   | 过滤条件   | 
| startTime   | 搜索时间区间   | datetime   | O   | 筛选条件   | 
| deadTime   | 搜索时间区间   | datetime   | O   | 筛选条件   | 

**出参：**

| 参数名 | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| data | 任务信息列表   | List of Object   |    |    | 
| ├─taskName   | 任务名称   | String   | M   |    | 
| ├─questionBankCode | 作业库代码   | String   | M   |    | 
| ├─taskDesc   | 任务描述   | String   | M   |    | 
| ├─createdAt   | 创建时间   | datetime   | M   |    | 
| ├─updatedAt   | 更新时间   | datetime   | M   |    | 
| ├─deadTime   | 截止时间   | datetime   | M   |    | 
| ├─taskCreateby   | 创建人   | String   | M   |    | 
| ├─taskStatus   | 任务状态   | number   | M   |    | 

示例：

### ![图片](https://uploader.shimo.im/f/Ey7PmHpuaeoiFW8j.png!thumbnail)

**后端逻辑：**

①接收：token、搜索信息

②数据库操作：根据token解析出的用户学工号，根据输入的搜索信息模糊查询数据库中符合要求的信息，返回作为显示。

### 2.2.2.8 题库修改

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| questionBankCode   | 作业库代码   | String   | M   |    | 
| questionType   | 题目类型   | String   | M   |    | 
| questionId   | 题目编号   | String   | M   |    | 
| itemDetail   | 题目详细内容   | Map   | M   | "选择题：题目选项，正确选项，选项数量","填空题：正确答案","代码题目：标准输入，标准输出"   | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**示例：**

![图片](https://uploader.shimo.im/f/0xiW2EdT1BkRfXTE.png!thumbnail)

**后端逻辑：**

①接收：token、作业库名称

②数据库操作：根据token解析出的用户学工号和接收的作业库名称，返回相应作业库的所有题目详情（录入的全部信息），修改后跳到录入接口

## 2.3 教师班级管理模块

### 2.3.1 创建班级

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| classCode   | 班级代号   | varchar(50)   | M   |    | 
| classCreatedBy   | 创建人   | varchar(50)   | M   |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**示例：**

![图片](https://uploader.shimo.im/f/bkLJXDdK1YAKz9qT.png!thumbnail)**后端逻辑：**

①接收：token，新建班级名称

②数据库操作：根据token解析出的用户学工号，根据输入的班级名称查询是否已存在，未存在则新建班级，在数据表中添加新班级，创建人，创建时间等信息。

③报错信息：该班级已存在，请更换新的名称。

### 2.3.2 退出班级

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| classCode | 班级代号   | varchar(50)   | M   |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**示例：**

![图片](https://uploader.shimo.im/f/ZwOulXj6jnk3I4F5.png!thumbnail)

**后端逻辑：**

①接收：token，班级名称

②数据库操作：根据token解析出的用户学工号，根据输入的班级名称，删除该学工号在该班级数据库中的信息。 

# **3.****学生管理模块****具体设计**

### 3.1 功能描述

对学生做题，答题，成绩查询进行数据库和接口管理

### 3.2 接口实现

### 3.2.1 获取任务列表

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| taskStatus   | 任务状态   | Number   | O   | 过滤条件   | 
| classCode   | 所在班级   | String   | M   | 学生只能查看自身班级的任务   | 

**出参**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| data   | 任务列表   | List of Object   |    |    | 
| ├─taskStatus   | 任务状态   | Number   | M   | 当前任务状态   | 
| ├─taskName   | 任务名称   | String   | M   | 过滤条件   | 
| ├─taskCreateby   | 创建人   | String   | M   | 创建人   | 
| ├─taskDesc   | 任务描述   | String   | M   | 任务描述   | 
| ├─createdAt   | 创建时间   | datetime   | M   | 发布时间   | 

示例：

![图片](https://uploader.shimo.im/f/haFAvMeZhj0KOiH0.png!thumbnail)

**后端逻辑：**

①接收：token

②数据库操作：根据token解析出的用户学工号，检索班级详情数据表，获得学生所在班级，检索任务数据表，返回此班级所有的任务及任务详情（发布者、任务描述、截止时间、任务状态、学生完成进度）列表

③处理的错误信息：未加入班级

### 3.2.2 获取历史作答情况

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| taskName   | 任务名称   | String   | M   |    | 
| studentCode   | 学工号   | String   | M   | 学生只能查看自己的成绩   | 

**出参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| data | 任务列表   | List of Map   |    |    | 
| ├─answerPoints   | 学生得分   | Number   | M   |    | 
| ├─questionId   | 题目编号   | String   | M   | 任务的题目编号   | 
| ├─├─itemAnswer   | 标准答案   | String/Int   | M   |    | 
| ├─├─studentAnswer   | 提交答案   | String/Int   | M   |    | 

示例：

![图片](https://uploader.shimo.im/f/heg7SSKkMoc0fokb.png!thumbnail)

### 3.2.3 获取任务详情（用于答题展示界面）

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| taskName   | 任务名称   | String   | M   |    | 

**出参：**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| data | 作业库信息列表   | List of Object   |    |    | 
| ├─taskName   | 任务名称   | String   | M   | 任务名称   | 
| ├─taskDesc   | 任务描述   | String   | M   | 任务描述   | 

示例：

### ![图片](https://uploader.shimo.im/f/uA4VsfPPvYIB6FaQ.png!thumbnail)

**后端逻辑部分：**

①接收：token、任务名称

②数据库操作：根据token解析出的用户学工号和接收的任务名称，检索任务数据表，获得任务对应的作业库名称，检索作业库详情数据表，返回此作业库中所有的题目详情（选择、填空、编程分三个列表返回）（如果题目录入的是图片，将图片base64编码后返回给前端）

***题目提交（未截止前可重复提交）：选择、填空、编程三个提交接口***

①接收：token、任务名称、提交答案（选择填空）、文件（编程）

②数据库操作：根据token解析出的用户学工号和接收的任务名称，在提交详情数据表中检索题目是否已经提交，未提交，在提交详情数据表中写入提交的答案情况，已提交，未到截止日期，更新答案情况。将提交的答案与标准答案比对，获得学生此题得分。对于编程题，后端接收python文件到指定文件夹，采用popen调用命令行的方法运行python文件，将得到的输出与测试输出比对，获得编程题得分。

***结束提交（未截止前可重复提交）***

①接收：token、任务名称

②数据库操作：根据token解析出的用户学工号和接收的任务名称，在提交详情数据表中检索此次任务此学生的所有提交记录，计算总得分，在提交数据表中检索任务是否已被提交，未提交，在提交数据表中写入此总得分，已提交，未截止，更新得分

③处理的错误信息：任务截止后，答案不可再提交

### 3.2.4 加入班级

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| classCode   | 班级代号   | String   | M   |    | 
| studentCode   | 学号   | String   | M   |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**示例：**

![图片](https://uploader.shimo.im/f/C4LMP9Cl8r8IwRQO.png!thumbnail)

**后端逻辑：**

①接收：token，班级名称

②数据库操作：根据token解析出的用户学工号，根据输入的班级名称，添加该学工号在该班级数据库中的信息。，象征着用户加入了该班级。

### 3.2.5 退出班级

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| classCode | 班级代号   | varchar(50)   | M   |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

**示例：**

![图片](https://uploader.shimo.im/f/ZwOulXj6jnk3I4F5.png!thumbnail)

**后端逻辑：**

①接收：token，班级名称

②数据库操作：根据token解析出的用户学工号，根据输入的班级名称，删除该学工号在该班级数据库中的信息。 

# 4.后台管理模块具体设计

## 4.1 功能描述

管理员对用户进行管理和统计查询修改等

## 4.2 接口实现

### 4.2.1 初始化用户列表

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----|:----|:----|:----|
| 无   |    |    |    |    | 

**出参：**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----:|:----|:----:|:----|:----:|:----|:----:|
| userlist   | 学生列表 | List of Object   | M   |    | 
| ├─key   | 序号   | varchar(50)   | M   |    | 
| ├─userCode   | 学号   | varchar(50)   | M   |    | 
| ├─userName   | 姓名   | varchar(50)   | M   |    | 
| ├─type   | 用户类型   | varchar(50)   | M   |    | 


主页初始化阶段获取后端数据库中的所有已存在的用户数据，分类做显示。

![图片](https://uploader.shimo.im/f/TVoN7qDhXyow1PsV.png!thumbnail)

### 4.2.2 获取统计信息

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----|:----|:----|:----|
| requestFlag   | 页面加载时响应   |    |    |    | 

**出参：**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----:|:----:|:----|:----:|:----|:----:|:----|:----:|
| data   | 学生列表 | List of Object   | M   |    | 
| ├─studentNumber   | 姓名   | varchar(50)   | M   | 过滤条件   | 
| ├─teacherNumber   | 学号 | varchar(50)   | M |    | 

示例：

![图片](https://uploader.shimo.im/f/8DfowF6NvwgmUXOx.png!thumbnail)

### 4.2.3  修改用户信息

**入参****：**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----:|:----|:----:|:----|:----:|:----|:----:|
| userInfo   | 学生列表 | List of Object   | M   |    | 
| ├─studentName   | 姓名   | varchar(50)   | M   |    | 
| ├─teacherCode | 学号 | varchar(50)   | M |    | 
| ├─type   | 账户类型   | varchar(50)   |    |    | 
| ├─phone   | 手机号   | varchar(50)   |    |    | 
| ├─email | 邮箱   | varchar(50)   |    |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 


示例：

![图片](https://uploader.shimo.im/f/L5n5utmVsz0MAbvG.png!thumbnail)

### 4.2.4 增加用户

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| adminCode   | 管理员账户   | varchar(50)   | M   | 管理员账户   | 
| userInfo   | 用户信息   | List of Object   | M   |    | 
| ├─code   | 学工号   | varchar(50)   | M   | 教师工号   | 
| ├─name   | 姓名   | varchar(50)   | M   | 教师姓名   | 
| ├─password   | 设置的密码   | varchar(50)   | M   | 口令的SHA-2指纹   | 
| ├─phone   | 手机号   | varchar(50)   | O   | 找回密码用   | 
| ├─email   | 邮箱   | varchar(50)   | O   | 找回密码用   | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "访问成功！" | 
| success | 请求是否成功 | booleans | M | true | 

示例：

![图片](https://uploader.shimo.im/f/JpwNWqRCfAYyKN6e.png!thumbnail)

### 4.2.5 初始化操作列表

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----:|:----:|:----:|:----:|:----:|:----:|:----|
| 无   |    |    |    |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----|
| opRecord | 操作列表 | List of Object | M |    | 
| ├─key | 操作id | booleans | M |    | 
| ├─adminCode | 管理员账号 | varchar(50)   | M |    | 
| ├─adminName | 管理员姓名 | varchar(50)   | M |    | 
| ├─type | 操作类型 | varchar(50)   | M |    | 
| ├─objCode | 目标账号 | varchar(50)   | M |    | 
| ├─opTime | 操作时间 | varchar(50)   | M |    | 
| admins   | 管理员列表   | List of Object   | M   |    | 
| ├─adminCode   | 管理员账号   | varchar(50)   | M   |    | 
| ├─adminName   | 管理员姓名   | varchar(50)   | M   |    | 

![图片](https://uploader.shimo.im/f/RnLzzLUB0G8dWHQ7.png!thumbnail)

**后端逻辑：**

数据库操作：根据接受的管理员账号，在admin数据表中检索所有的操作记录，将检索结果以列表形式返回（操作者、操作时间、操作类型、操作对象），同时返回初始化页面需要的管理员信息（账号、姓名）。

### 4.2.6 获取操作列表

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----|
| opType   | 操作类型   | varchar(50)   | M   |    | 
| opName   | 管理员姓名   | varchar(50)   | M   |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----|:----|:----|:----:|:----|
| opRecord   | 操作列表   | List of Object   | M |    | 
| ├─key   | 操作id   | booleans | M |    | 
| ├─adminCode   | 管理员账号   | varchar(50)   | M   |    | 
| ├─adminName   | 管理员姓名   | varchar(50)   | M   |    | 
| ├─type   | 操作类型   | varchar(50)   | M   |    | 
| ├─objCode   | 目标账号   | varchar(50)   | M   |    | 
| ├─opTime   | 操作时间   | varchar(50)   | M   |    | 

![图片](https://uploader.shimo.im/f/XD7zdmfDRB439nyA.png!thumbnail)

### 4.2.7 删除操作

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
| opTag   | 唯一识别码   | varchar(50)   | M   | 用于识别哪条记录   | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----|
| message | 异常消息说明 | String | M | "删除成功"   | 
| success | 请求是否成功 | booleans | M | true | 

### ![图片](https://uploader.shimo.im/f/PPy5kJXZ9LUUxXMO.png!thumbnail)

### 4.2.8  查找用户

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----|:----|:----|:----|
| studentCode   | 学工号   | varchar(50)   | M   |    | 

**出参：**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----:|:----:|:----|:----:|:----|:----:|:----|:----:|
| data   | 学生列表 | List of Object   | M   |    | 
| ├─studentName   | 姓名   | varchar(50)   | M   |    | 
| ├─teacherCode | 学号 | varchar(50)   | M |    | 
| ├─type   | 账户类型   | varchar(50)   |    |    | 
| ├─phone   | 手机号   | varchar(50)   |    |    | 
| ├─email | 邮箱   | varchar(50)   |    |      | 

![图片](https://uploader.shimo.im/f/wD3bJvvpzwAu1s1z.png!thumbnail)

### 4.2.9 获取用户列表

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----|:----|:----|:----|
| type   | 用户类型   | varchar(50)   | M   | 默认为全部   | 
| name   | 用户姓名   | varchar(50)   | M   | 默认为全部   | 
| code   | 用户学号   | varchar(50)   | M   | 默认为全部   | 

**出参：**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----:|:----:|:----|:----:|:----|:----:|:----|:----:|
| userlist | 学生列表 | List of Object   | M   |    | 
| ├─key | 序号 | varchar(50)   | M   |    | 
| ├─userCode   | 学号   | varchar(50)   | M   |    | 
| ├─userName   | 姓名   | varchar(50)   | M   |    | 
| ├─type   | 用户类型   | varchar(50)   | M   |    | 

![图片](https://uploader.shimo.im/f/3CbAfoznV0YjIx6k.png!thumbnail)

### 4.2.10 删除用户

**入参:**

| 参数名   | 参数描述   | 数据类型   | 必填   | 示例/备注   | 
|:----|:----|:----|:----|:----|
| adminCode   | 管理员账号   | varchar(50)   | M   |    | 
| userCode   | 用户姓名   | varchar(50)   | M   |    | 
| userType   | 用户类型   | varchar(50)   | M   |    | 

**出参：**

| 参数名 | 参数描述 | 数据类型 | 必填 | 示例/备注 | 
|:----:|:----:|:----:|:----:|:----:|
| message | 异常消息说明 | String | M | "删除成功" | 
| success | 请求是否成功 | booleans | M | true | 

![图片](https://uploader.shimo.im/f/EFiaOQQ1yq8TYOiK.png!thumbnail)





# 5.开发过程出现的问题

### 5.1 前端部分

**5.1.1.对于 react中 function 和 class的区别**：

![图片](https://uploader.shimo.im/f/BYr0QPztGTkAOjQo.png!thumbnail)

① functional component语法更简单，只需要传入一个props参数，返回一个react片段。

class component 要求先继承React.Component然后常见一个render方法，在render里面返回react片段。

② state 状态：因为function component 知识一个普通的函数所以不可以在其中用this.state , setState(),这也是它被叫做无状态组件的原因。所以一个组件需要用到状态的时候要用到class component。

③ lifecycle hooks 生命周期：function component 不具有生命周期，因为所有的生命周期钩子函数均来自于React.Component。所以当一个组件需要生命周期钩子的时候我们也需要class component。

**5.1.2.react中实现跳转传参的方式：**

**① DOM跳转**

在需要跳转的页面导入import {Link} from 'react-router-dom'，在需要跳转的地方使用link标签的to属性进行跳转，路由配置文件中导出的那个类名相当于相当于router-view标签，在需要展示的地方引入这个类进行展示。（需要提前配置路由才能进行展示）

![图片](https://uploader.shimo.im/f/upNhBUbfYxQ28V2v.png!thumbnail)
**② ****js跳转**

使用this.props.history.push('/child02')

但是经过观察发现这种方法很难实现this.props.history在子组件和父组件之间的传递，对于分层很明显的结构 在第二层往下很难使用。

![图片](https://uploader.shimo.im/f/lLDwhK6G3SsYt0mZ.png!thumbnail)
**2、路由的传参**

一、params传参

1、在路由配置中以/：的方式评接参数标识

 ![图片](https://uploader.shimo.im/f/bwRKhjl6NXcJeTbM.png!thumbnail)

 

 2、在路径后面将参数评接上(/参数)

 

![图片](https://uploader.shimo.im/f/2LFm7juw9dIg16ZH.png!thumbnail)

 3、在被跳转页使用this.props.match.params.xxx(此处为id)    接收参数

![图片](https://uploader.shimo.im/f/x4A6FL10JNkDbeBy.png!thumbnail)

二、query传参

 ![图片](https://uploader.shimo.im/f/iYBhhQMoo8Uob9ej.png!thumbnail)

1、在router文件中配置为正常配置   <Route path="/Child03" component={Child03}/>

2、在跳转时  路径为一个对象{}     其中 pathname为路径  query为一个对象  对象里是携带的参数

3、使用this.props.location.query接收参数

三、state传参

使用this.props.location.state接收参数

 ![图片](https://uploader.shimo.im/f/xHRX0KNAXA47u8f8.png!thumbnail)

![图片](https://uploader.shimo.im/f/1IrwTGNiyeouEo9z.png!thumbnail)  

5.1.3 GET  POST请求区别：

大体总结为：（本质上是一次请求和二次请求的区别）

使用GET方法：客户端与服务端的交互像是一个提问(如查询操作、搜索操作、读操作) 

使用POST方法： 

       1.交互是一个命令或订单(order)，比提问包含更多信息 

       2.交互改变了服务器端的资源并被用户察觉，例如订阅某项服务 

       3.用户需要对交互产生的结果负责 

5.1.4 一个界面中的组件通信

解决方案1：让有关联的模块用父子关系连接，父组件的状态可以作为子组件的参数传入。父组件setstate函数也会同时导致子组件重新渲染，完成参数传递。优点，依然遵守按模块开发的设计初心。缺点，传递参数只能单向传递。

解决方案2：各组件合成为一个大的组件，共享一个state，用布局管理器分行分块显示。优点，易于传递参数，修改state后会全局重新渲染。缺点，修改其中一个分块会导致整体布局发生比较大的变化，看起来很不专业。

解决方案3：修改全局变量，各模块不断监听全局变量，当全局变量变化后，实现重新渲染。缺点，浪费计算资源。优点，不需要大规模修改代码。

最终综合各种原因，采用解决方案1。

### 5.2 后端部分

**5.2.1.****     ****前后端传输信息不通，添加了请求头信息后没有解决问题**

>前后端通信传输的信息为json编码格式，传输数据在请求的body里，后端将接收请求的body部分进行json解码后，才可以获得前端发送的数据信息，后端向前端返回信息时，数据也要用json格式进行编码。

![图片](https://uploader.shimo.im/f/L4hGFLEmvTwbMacP.png!thumbnail)

**5.2.2.****     ****录入图片接收存储的问题**

>后端接收前端发送的base64编码后的图片数据，对这些数据进行base64解码，获得传送的图片，将图片保存到指定文件夹，前端需要显示图片题目时，后端将图片再进行一次base64编码后传给前端。

![图片](https://uploader.shimo.im/f/ALF24A2adSAFUDsN.png!thumbnail)

![图片](https://uploader.shimo.im/f/KFVZwQM6v1gIfN1U.png!thumbnail)

**5.2.3.****     ****编程题python文件接收错误、无法存储和执行**

>用txt文件进行实验时，字节传输，后端需要将其进行解码转换成字符串再写入文件，要求后端解码时支持的编码方式必须和前端一致（同为utf-8或其他），接收错误（接收不到、存储不到文件夹或者接收到文件内容打印出来出现空格）是编码方式不同导致的。python文件接收后，存储到指定文件夹。

![图片](https://uploader.shimo.im/f/A8kvCKKVh9cbFpLJ.png!thumbnail)

>**执行python文件**：

①调用os库的**system函数**获取执行cmd命令，在cmd里输出的内容会直接在编译器控制台输出。但system函数相当于从当前进程中打开一个子进程来执行命令，函数返回除了输出结果外一定会附带一个返回状态码，返回0表示函数执行成功，是一种比较简单粗暴的调用命令行的方式。

![图片](https://uploader.shimo.im/f/czDEL8JMSXUXJUHw.png!thumbnail)

![图片](https://uploader.shimo.im/f/UuANmv6eQBkv1Ruu.png!thumbnail)

②使用os库的**popen函数**调用命令行执行python文件。popen函数可以调用cmd执行python文件并返回文件输出结果，返回的结果是一个file对象，结果需要像读文件一样读取才能得到。与system函数的区别是popen函数是从命令打开一个管道，获取输入到cmd控制台的信息，如果执行成功，不会返回状态码（直接读取输出结果与测试输出比较可得分数，适合需求），执行失败会返回一个空字符串。相比前者更加有效。

![图片](https://uploader.shimo.im/f/q0iKnic6HVcRaBxC.png!thumbnail)

**5.2.4.****     ****存储文件/图片到指定文件夹，发现总是存储失败**

>存储到具体文件夹路径末尾要加\\，表示进入此文件夹

![图片](https://uploader.shimo.im/f/UBRoyz9uGYACNa5L.png!thumbnail)

**5.2.5.****     ****联调的时候****有些部分报错****ValueError: The QuerySet value for an exact lookup must be limited to one result using slicing**

>数据表的索引信息必须是确定信息而不能直接是数据表的检索结果，对数据表filter过滤筛选后返回的是列表结果，需要选取列表中结果的某一确定字段再进行操作。

![图片](https://uploader.shimo.im/f/I60aadmSTaQ1APxX.png!thumbnail)

**5.2.6****.   **** 对提交代码安全性的检验**

>存在问题：作业自动评估平台会评估python代码，python代码需要运行后才能进行评估。python代码具有很高的权限，可能会干扰系统的正常运行。

>解决方法：

① 解决方案1：对所执行文件进行关键字检验，凡是包含import的语句，检测其import的内容。涉及到规定之外的内容禁止执行。

优点是不会影响正常代码的执行，学生提交代码不会感知到检验系统的存在。

缺点是如果知道检验原理依然可以用各种办法绕开检测，防不胜防。

② 解决方案2：由老师指定所用到的库文件，规定学生提交的代码不允许有import字样，由老师设置合法的import库，平台自动添加代码。

优点，比较彻底的解决了高权限库的添加。

缺点，无法避免其他不使用系统库的恶意操作。

③ 解决方案3：内嵌专业自定义校验器。比如竞赛常用的cena评测器，已经解决了代码安全问题。内嵌cena评测器，用该评测器进行代码检测，可以解决安全问题。

# 6.个人总结

见单独的附件-个人总结

