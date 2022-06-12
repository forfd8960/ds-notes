# Raft concepts

Raft is a consensus algorithm.

R{eliable|eplicated|edundant} And Fault-Tolerant -> Raft


## 系统模型

* 服务器可能宕机，停止运行，过段时间再恢复
* 消息可能丢失、延迟、乱序活重复，可能有网路分区，并在一段时间后恢复.

## 基本概念

* Leader: 领导者负责所有客户端请求和日志复制
* Follower: 完全被动的处理请求，Folloer 不主动发送 RPC 请求，只响应收到的 RPC 请求, 服务器大多数情况下处于此状态.
* Candidate: 候选者用来选举出新的领导者，候选者是处于 Leader 和 follower 之间的暂时状态.

```go

const (
    Follower = iota
    Candidate
    Leader
)
```

Raft 选出 Leader 意味着进入一个新的任期(Term), 任期就是一个逻辑时间. 每一个任期都由一个数字来表示任期号.

* CurrentTerm: 服务器当前已知的最新任期号.
* 服务器之间通信通过 RequestVote RPC, AppendEntries RPC 通信.
* RequestVote RPC: 用于领导者选举.
* AppendEntries RPC: Leader 用来复制日志和发送心跳.

## 