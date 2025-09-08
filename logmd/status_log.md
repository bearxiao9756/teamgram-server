# status 日志分析

## 初始化
```
{"@timestamp":"2025-09-07T14:08:30.939Z","caller":"commands/root.go:70","content":"instance initialize...","level":"info"}
```

## 启动中 
```
{"@timestamp":"2025-09-07T14:08:30.940Z","caller":"server/server.go:38","content":{"Name":"service.status","Log":{"ServiceName":"","Mode":"file","Encoding":"json","TimeFormat":"","Path":"../logs/status","Level":"debug","MaxContentLength":0,"Compress":false,"Stat":true,"KeepDays":0,"StackCooldownMillis":100,"MaxBackups":0,"MaxSize":0,"Rotation":"daily","FileTimeFormat":""},"Mode":"pro","MetricsUrl":"","Prometheus":{"Host":"","Port":0,"Path":""},"Telemetry":{"Name":"","Endpoint":"","Sampler":0,"Batcher":"","OtlpHeaders":null,"OtlpHttpPath":"","OtlpHttpSecure":false,"Disabled":false},"DevServer":{"Enabled":false,"Host":"","Port":0,"MetricsPath":"","HealthPath":"","EnableMetrics":false,"EnablePprof":false,"HealthResponse":""},"Shutdown":{"WrapUpTime":0,"WaitTime":0},"Profiling":{"Name":"","ServerAddr":"","AuthUser":"","AuthPassword":"","UploadRate":0,"CheckInterval":0,"ProfilingDuration":0,"CpuThreshold":0,"ProfileType":{"Logger":false,"CPU":false,"Goroutines":false,"Memory":false,"Mutex":false,"Block":false}},"ListenOn":"0.0.0.0:20670","Etcd":{"Hosts":["etcd:2379"],"Key":"service.status","ID":0,"User":"","Pass":"","CertFile":"","CertKeyFile":"","CACertFile":"","InsecureSkipVerify":false},"Auth":false,"Redis":{"Host":"","Type":"","User":"","Pass":"","Tls":false,"NonBlock":false,"PingTimeout":0,"Key":""},"StrictControl":false,"Timeout":2000,"CpuThreshold":900,"Health":true,"Middlewares":{"Trace":true,"Recover":true,"Stat":true,"StatConf":{"SlowThreshold":0,"IgnoreContentMethods":null},"Prometheus":true,"Breaker":true},"MethodTimeouts":null,"Status":[{"Host":"redis:6379","Type":"node","User":"","Pass":"","Tls":false,"NonBlock":true,"PingTimeout":1000000000,"Weight":100}],"StatusExpire":90},"level":"info"}
```
### 核心配置参数解析

#### 服务基本信息
* @timestamp: 2025-09-07T14:08:30.939Z 和 2025-09-07T14:08:30.940Z，表示日志记录的时间戳。

* level: "info"，表明这是一个信息级别的日志，通常用于记录正常的操作或重要事件。

* content: 包含服务的具体配置信息。

* Name: "service.status"，这是该服务的名称。

* ListenOn: "0.0.0.0:20670"，表示服务正在监听所有网络接口上的 20670 端口。

#### 日志

 Log: 这部分定义了服务自身的日志记录设置。

* Mode: "file"，日志将写入文件。

* Encoding: "json"，日志内容使用 JSON 格式编码。

* Path: "../logs/status"，日志文件的存储路径。

* Level: "debug"，日志记录级别设置为调试，这意味着它会记录所有级别的日志，包括调试信息。

* Rotation: "daily"，日志文件将按天进行轮换。

#### 注册与发现
Etcd: 这部分配置了服务如何使用 etcd 进行服务注册和发现。etcd 是一个分布式键值存储系统，常用于配置管理和服务发现。
* Hosts: ["etcd:2379"]，etcd 服务的地址是 etcd，端口是 2379。
* Key: "service.status"，服务在 etcd 中注册的键名

#### 中间件配置
Middlewares: 定义了服务中启用的中间件
* Trace: true，启用链路追踪中间件
* Recover: true，启用宕机恢复中间件，防止服务因 panic 而崩溃
* Stat: true，启用统计中间件，用于收集性能数据。
* Prometheus: true，启用 Prometheus 中间件，用于暴露监控指标
* Breaker: true，启用断路器中间件，用于保护服务免受依赖服务故障的影响

#### 健康检查与状态

* Health: true，表示启用了健康检查
* Status: 这是一个数组，包含服务依赖的其他服务的状态信息
```
Host: "redis:6379"，依赖的 Redis 服务地址。

Type: "node"，表示这是一个节点类型。

PingTimeout: 1000000000，ping 超时时间为 10 亿纳秒，即 1 秒。

StatusExpire: 90，状态信息的有效期为 90 秒。
```

#### 其他配置
* Auth: false，表示服务没有启用身份验证
* Redis: Redis 部分的配置字段为空，但在 Status 中有 Redis 服务的健康检查配置，这可能意味着服务的核心逻辑不直接使用 Redis，但会监控其可用性。
* Timeout: 2000，通用超时时间为 2000 毫秒
* CpuThreshold: 900，CPU 使用率阈值为 900（可能表示千分之 900，即 90%）