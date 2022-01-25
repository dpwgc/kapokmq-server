# KapokMQ - 轻量级分布式消息队列 - 下载

## KapokMQ - Serena 服务器应用整合包

***

#### KapokMQ - 消息队列源码

* https://github.com/dpwgc/kapokmq

* https://gitee.com/dpwgc/kapokmq

***

#### Serena - 服务发现与注册中心源码

* https://github.com/dpwgc/serena

* https://gitee.com/dpwgc/serena

***

#### kapokmq-go-client - Golang客户端

* https://github.com/dpwgc/kapokmq-go-client

* https://gitee.com/dpwgc/kapokmq-go-client

***

### 目录说明

##### /kapokmq-server

* /kapokmq-server
  * /kapokmq-server-linux `Linux版本`
    * /kapokmq `消息队列`
    * /serena `注册中心` 
  * /kapokmq-server-windows `Windows版本`
    * /kapokmq `消息队列`
    * /serena `注册中心`

```yaml
/kapokmq                      # 文件根目录
    kapokmq.exe               # Windows exe启动文件
    kapokmq                   # Linux二进制文件
    run.sh                    # Linux启动脚本
    /config                   # 配置目录
        application.yaml      # 消息队列配置文件
    /log                      # 日志存储目录
    /view                     # 前端-Vue项目打包文件
    MQDATA                    # 持久化文件
```

```yaml
/serena                       # 文件根目录
    serena.exe                # Windows exe启动文件
    serena                    # Linux二进制文件
    run.sh                    # Linux启动脚本
    /config                   # 注册中心配置目录
        application.yaml      # 配置文件
    /log                      # 日志存储目录
```

***

### 部署方式

* 本应用无需安装任何依赖，开箱即用。

* 服务器需要开放对应的端口号（Gin http服务端口号、Gossip服务端口号）。

* 推荐使用默认配置，可在默认配置的基础上调整。

* 消息队列与注册中心的安全访问密钥secretKey应保持一致。

* 在非内网服务器集群上部署时，需将配置文件中的Addr地址改服务器公网IP。

#### 在Linux上部署项目

##### 将 kapokmq-server-linux 文件夹上传至服务器

* 单机部署方式

```yaml
# 运行kapokmq
cd到 /kapokmq
配置 kapokmq 的 config/application.yaml
终端执行 ./run.sh
```

* 集群部署方式

```yaml
# 运行serena
cd到 /serena
配置 serena 的 config/application.yaml
终端执行 ./run.sh

# 运行kapokmq
cd到 /kapokmq
配置 kapokmq 的 config/application.yaml
终端执行 ./run.sh

# 注：如果采用集群方式部署，要先运行注册中心，再运行消息队列
```

#### 在Windows上部署项目

##### 将 kapokmq-server-windows 文件夹上传至服务器

* 单机部署方式

```yaml
# 运行kapokmq
cd到 /kapokmq
配置 kapokmq 的 config/application.yaml
运行 kapokmq.exe
```

* 集群部署方式

```yaml
# 运行serena
cd到 /serena
配置 serena 的 config/application.yaml
运行 serena.exe

# 运行kapokmq
cd到 /kapokmq
配置 kapokmq 的 config/application.yaml
运行 kapokmq.exe

# 注：如果采用集群方式部署，要先运行注册中心，再运行消息队列
```

#### 消息队列网页端控制台
```yaml
# 消息队列启动后访问：
http://localhost:port/#/Console
```

***

### 配置说明

#### KapokMQ消息队列配置： ./kapokmq/config/application.yaml

```yaml
server:
  # ip地址/域名
  addr: 0.0.0.0
  # Gin服务运行端口号
  port: 8011

mq:
  # 生产者、控制台访问密钥（放在请求头部）
  secretKey: test

  # 消息推送模式（1：点对点模式，一条消息只能随机被一个消费者客户端消费。2：订阅/发布推送模式：将消息推送给所有消费者客户端）
  pushType: 1

  # 消息通道的缓冲空间大小（消息队列的容量）
  messageChanBuffer: 10000000

  # 推送消息的速度（{pushMessagesSpeed}秒/一批消息）
  pushMessagesSpeed: 1
  # 单批次推送的消息数量
  pushCount: 10000
  # 消息推送失败后的立即重试的次数
  pushRetryCount: 3

  # 持久化文件
  persistentFile: MQDATA
  # 是否进行持久化（1：是。0：否）
  isPersistent: 1
  # 数据恢复策略（0：清空本地数据，不进行数据恢复。1：将本地数据恢复到内存）
  recoveryStrategy: 1
  # 两次持久化的间隔时间（单位：秒）
  persistentTime: 3

  #是否立即清除已确认消费的消息（1：是。0：否）
  isCleanConsumed: 0

  # 是否开启自动重推未确认消费消息功能（1：是。0：否）
  isRePush: 1
  # 是否开启自动清理过期消息功能（1：是。0：否）
  isClean: 1

  # 检查消息的速度（每隔{checkSpeed}秒检查一批消息，用于消费失败的消息重推、延时消息推送与过期消息清理）
  checkSpeed: 3

  # 消息过期阈值（当消息存在超过{cleanTime}秒后，删除该消息）
  cleanTime: 259200

# Gossip集群配置
cluster:
  # 是否以集群方式部署（1：是。0：否）
  isCluster: 0
  # 该节点的Gossip服务端口号（使用Gossip协议，通过此端口连接注册中心，不能与上面的Gin http服务端口号{server.port}相同）
  gossipPort: 8021
  # 注册中心的ip地址/域名
  registryAddr: 0.0.0.0
  # Serena注册中心的Gossip服务端口号
  registryGossipPort: 8041
```

#### Serena注册中心配置： ./serena/config/application.yaml

```yaml
server:
  # ip地址/域名
  addr: 0.0.0.0
  # Gin服务端口号
  port: 8031

registry:
  # 安全密钥
  secretKey: test
  # Gossip服务运行端口号
  gossipPort: 8041
```