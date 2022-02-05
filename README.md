# KapokMQ

## KapokMQ & Serena 服务器应用整合包

![Go](https://img.shields.io/static/v1?label=LICENSE&message=Apache-2.0&color=orange)
![Go](https://img.shields.io/static/v1?label=Go&message=v1.17&color=blue)
[![github](https://img.shields.io/static/v1?label=Github&message=kapokmq&color=blue)](https://github.com/dpwgc/kapokmq)
[![star](https://gitee.com/dpwgc/kapokmq/badge/star.svg?theme=dark)](https://gitee.com/dpwgc/kapokmq/stargazers)
[![fork](https://gitee.com/dpwgc/kapokmq/badge/fork.svg?theme=dark)](https://gitee.com/dpwgc/kapokmq/members)

#### KapokMQ ~ 消息队列源码

* https://github.com/dpwgc/kapokmq `github`

* https://gitee.com/dpwgc/kapokmq `gitee`

#### Serena ~ 服务发现与注册中心源码

* https://github.com/dpwgc/serena `github`

* https://gitee.com/dpwgc/serena `gitee`

#### kapokmq-go-client ~ Golang客户端

* https://github.com/dpwgc/kapokmq-go-client `github`

* https://gitee.com/dpwgc/kapokmq-go-client `gitee`

***

### 目录说明

##### /kapokmq-server

* /kapokmq-server
  * /kapokmq `消息队列`
  * /serena `注册中心`

```yaml
/kapokmq                      # 文件根目录
    kapokmq.exe               # Windows exe启动文件
    kapokmq                   # Linux二进制文件
    run.sh                    # Linux启动脚本
    application.yaml          # 消息队列配置文件
    /log                      # 日志存储目录
    /view                     # 前端-Vue项目打包文件
    MQDATA                    # 持久化文件
```

```yaml
/serena                       # 文件根目录
    serena.exe                # Windows exe启动文件
    serena                    # Linux二进制文件
    run.sh                    # Linux启动脚本
    application.yaml          # 注册中心配置文件
    /log                      # 日志存储目录
```

***

### 注意事项

* 本应用无需安装任何依赖，开箱即用。

* Linux服务器上部署需给予 kapokmq-server-linux 文件目录777权限。

* 服务器需要开放对应的端口号（Gin http服务端口号、Gossip服务端口号）。

* 推荐使用默认配置，可在默认配置的基础上调整。

* 消息队列与注册中心的安全访问密钥secretKey应保持一致。

* 在非内网服务器集群上部署时，需将消息队列与注册中心配置文件中的Addr地址改服务器公网IP。

***

### 在Linux上部署项目

##### 将 kapokmq-server-linux 文件夹上传至服务器

* 单机部署方式

```yaml
# 运行kapokmq
cd到 /kapokmq
按需修改配置 ./application.yaml
终端执行 ./run.sh 启动服务
```

* 集群部署方式

```yaml
# 运行serena
cd到 /serena
按需修改配置 ./application.yaml
终端执行 ./run.sh 启动服务

# 运行kapokmq
cd到 /kapokmq
按需修改配置 ./application.yaml（isCluster需设为1）
终端执行 ./run.sh 启动服务

# 注：如果采用集群方式部署，要先运行注册中心，再运行消息队列
```

***

### 在Windows上部署项目

##### 将 kapokmq-server-windows 文件夹上传至服务器

* 单机部署方式

```yaml
# 运行kapokmq
cd到 /kapokmq
按需修改配置 ./application.yaml
运行 kapokmq.exe 启动服务
```

* 集群部署方式

```yaml
# 运行serena
cd到 /serena
按需修改配置 ./application.yaml
运行 serena.exe 启动服务

# 运行kapokmq
cd到 /kapokmq
按需修改配置 ./application.yaml（isCluster需设为1）
运行 kapokmq.exe 启动服务

# 注：如果采用集群方式部署，要先运行注册中心，再运行消息队列
```

***

### KapokMQ消息队列网页端控制台

##### 消息队列启动后访问：

> http://127.0.0.1:8011/#/Console

* 控制台需输入访问密钥登录，默认访问密钥为"test"，可在配置文件中修改。

***

### Serena注册中心开放接口

##### 获取所有消息队列节点信息

* 启动注册中心后，可通过此接口获取消息队列节点列表。

> http://127.0.0.1:8031/Registry/GetNodes

##### 请求方式
> POST

##### Content-Type
> form-data

##### 请求Header参数

| 参数        | 示例值   | 是否必填   |  参数描述  |
| :--------   | :-----  | :-----  | :----  |
| secretKey     | test |  必填 | 安全访问密钥 |

##### 成功响应

```json
[
	{
		"Name": "mq:123.123.123.111:8011",
		"Addr": "123.123.123.111",
		"Port": "8011"
	},
	{
		"Name": "mq:123.123.123.222:8011",
		"Addr": "123.123.123.222",
		"Port": "8011"
	}
]
```

***

### 配置说明

##### KapokMQ消息队列配置： ./kapokmq/application.yaml

##### Serena注册中心配置： ./serena/application.yaml