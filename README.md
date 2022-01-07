# kapokmq-server

***

### KapokMQ . 消息队列

##### https://github.com/dpwgc/kapokmq

##### https://gitee.com/dpwgc/kapokmq

***

### Serena . 注册中心

##### https://github.com/dpwgc/serena

#####https://gitee.com/dpwgc/serena

***

### kapokmq-go-client . Golang客户端

##### https://github.com/dpwgc/kapokmq-go-client

##### https://gitee.com/dpwgc/kapokmq-go-client

***

### 目录说明

##### /kapokmq-server 包含两个应用

* /kapokmq-server
  * /kapokmq # 消息队列
  * /serena # 注册中心（集群模式时使用）

```yaml
/kapokmq                  # 文件根目录
    kapokmq.exe           # Windows exe启动文件
    kapokmq               # Linux二进制文件
    run.sh                # Linux启动脚本
    /config               # 配置目录
        application.yaml  # 配置文件
    /log                  # 日志目录
    /view                 # 前端-Vue项目打包文件
    MQDATA                # 持久化文件
```

```yaml
/serena                   # 文件根目录
    serena.exe            # Windows exe启动文件
    serena                # Linux二进制文件
    run.sh                # Linux启动脚本
    /config               # 配置目录
    application.yaml      # 配置文件
    /log                  # 日志目录
```

### 部署方式

```yaml
# 在Linux上部署项目

# 单机部署方式
配置 kapokmq 的 application.yaml

cd到 /kapokmq
终端执行 ./run.sh

# 集群部署方式
配置 kapokmq 与 serena 的 application.yaml
cd到 /serena
终端执行 ./run.sh

cd到 /kapokmq
终端执行 ./run.sh

# 注：如果采用集群方式部署，要先运行注册中心，再运行消息队列
```

```yaml
# 在Windows上部署项目

# 单机部署方式
cd到 /kapokmq
运行 kapokmq.exe

# 集群部署方式
配置 kapokmq 与 serena 的 application.yaml

cd到 /serena
运行 serena.exe

cd到 /kapokmq
运行 kapokmq.exe

# 注：如果采用集群方式部署，要先运行注册中心，再运行消息队列
```

* 如果要在不同服务器间部署项目，需在kapokmq和serena的application.yaml中配置Addr地址为服务器公网IP。