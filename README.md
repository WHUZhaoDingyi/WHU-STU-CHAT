# WHU-STU-CHAT
C#大作业团队仓库——武汉大学学生互助交流平台

## 版本信息
- **当前版本**: V-0.0.2T (测试版)
- **更新时间**: 2025-4-28
- **更新人**: Noteles

## todo
1. 私聊功能
2. 好友功能
3. 个人信息
4. 群聊的搜索 与ai的对接 与文件系统和emoji的结合 与好友系统的结合 删除时删除或保留消息记录...

## 更新内容（4-28）
1. 完成并测试了群聊的后端与后端
2. 聊天成员表删除了1列
3. 融入框架了

## 更新内容（4-28）
1.实现AI对话咨询板块
2.实现AI接入聊天室进行总结
3.聊天室支持多媒体聊天（包含表情包/图片）
4.实现主页基础架构

## 技术栈
- **前端**: Vue.js + SignalR客户端
- **后端**: ASP.NET Core + SignalR+OpenAI
- **数据库**: MySQL+Redis

## 使用说明
1. 用户需先注册账号并登录
2. 登录后可进入公共聊天室与其他用户交流
3. 在聊天室可发送消息、查看在线用户
# 校园内部聊天平台功能说明

## 1. 聊天功能

### a) 私聊（个人聊天）
- **功能**：用户可以与其他用户进行一对一的私密交流，支持发送文字、图片、文件等。
- **消息状态**：支持“未读”和“已读”状态，用户可随时查看消息是否已阅读。
- **通知功能**：收到新私信时，系统会及时通知，确保用户不会错过任何重要信息。

### b) 群聊（群组聊天）
- **功能**：用户可以创建群聊，设定群聊名称、头像、描述，并邀请其他用户加入。
- **群成员管理**：群主可以管理群成员，设置管理员权限、移除成员等。
- **消息推送**：群聊消息实时同步，群成员可以随时查看并响应消息。
- **文件共享**：群聊支持图片、文件等多种形式的附件发送，方便分享学习资料。
- **群消息搜索**：支持根据时间、关键词等快速检索群聊中的历史消息。

### c) 公域聊天（开放聊天室）
- **功能**：开放多个聊天室，供用户参与不同主题的公共讨论。
- **聊天室**：如“校园公告区”、“活动讨论区”等，用户可以自由进入参与话题讨论。
- **实时消息**：支持文字、图片、文件的实时消息传递，增强互动性。
- **话题标签**：每条消息可以加上话题标签，方便后续分类和检索。

---




1. 用户表

| 字段名          | 数据类型         | 说明            |
| ------------ | ------------ | ------------- |
| UserId       | INT          | 主键，用户唯一标识     |
| Username     | VARCHAR(50)  | 用户名           |
| PasswordHash | VARCHAR(255) | 密码的哈希值        |
| Email        | VARCHAR(100) | 邮箱            |
| ProfilePic   | VARCHAR(255) | 头像 URL        |
| Role         | VARCHAR(20)  | 用户角色（如用户、管理员） |
| CreatedAt    | DATETIME     | 用户注册时间        |
| LastLogin    | DATETIME     | 最后登录时间        |

2. 聊天表（私聊、群聊和公域聊天）

| 字段名         | 数据类型         | 说明                                 |
| ----------- | ------------ | ---------------------------------- |
| ChatId      | INT          | 主键，聊天唯一标识                          |
| ChatName    | VARCHAR(100) | 聊天名称（用于群聊）                         |
| ChatType    | VARCHAR(20)  | 聊天类型（如 "private"、"group"、"public"） |
| CreatedBy   | INT          | 外键，创建者的 UserId                     |
| CreatedAt   | DATETIME     | 创建时间                               |
| Description | TEXT         | 群聊描述（群聊时使用）                        |

3. 消息表

| 字段名         | 数据类型        | 说明                            |
| ----------- | ----------- | ----------------------------- |
| MessageId   | INT         | 主键，消息唯一标识                     |
| ChatId      | INT         | 外键，关联到聊天表                     |
| SenderId    | INT         | 外键，发送者的 UserId                |
| Message     | TEXT        | 消息内容                          |
| MessageType | VARCHAR(20) | 消息类型（如 "text"、"image"、"file"） |
| SentAt      | DATETIME    | 发送时间                          |


4. 用户群聊关联表

记录每个用户与不同聊天的关联，用于群聊和私聊的成员管理

| 字段名      | 数据类型     | 说明             |
| -------- | -------- | -------------- |
| UserId   | INT      | 外键，用户的 UserId  |
| ChatId   | INT      | 外键，聊天的 ChatId  |
| JoinDate | DATETIME | 加入聊天的时间        |
| IsAdmin  | BOOLEAN  | 是否是管理员（仅对群聊有效） |

8. 文件表

| 字段名        | 数据类型         | 说明                       |
| ---------- | ------------ | ------------------------ |
| FileId     | INT          | 主键，文件唯一标识                |
| FileName   | VARCHAR(255) | 文件名                      |
| FilePath   | VARCHAR(255) | 文件存储路径                   |
| FileType   | VARCHAR(50)  | 文件类型（如 "image", "pdf" 等） |
| Size       | INT          | 文件大小（字节数）                |
| UploadedBy | INT          | 外键，上传者的 UserId           |
| UploadedAt | DATETIME     | 上传时间                     |
| ChatId     | INT          | 外键，关联到聊天表                |

9. 通知表（记录用户接收到的系统通知，如新消息提醒等）

|字段名|数据类型|说明|
|---|---|---|
|NotificationId|INT|主键，通知唯一标识|
|UserId|INT|外键，接收通知的 UserId|
|Content|TEXT|通知内容|
|IsRead|BOOLEAN|是否已读|
|CreatedAt|DATETIME|通知创建时间|
