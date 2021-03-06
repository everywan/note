# 分布式事务

本文假定读者了解数据库事务与分布式系统的相关知识.

关于数据库事务
1. 数据库本地事务的定义与场景
2. 本地事务的ACID特性
3. 建议了解本地事务的基本实现(参考innoDB即可)

如果有什么不了解的, 请参考 [事务](./transaction.md)

分布式事务自然涉及到分布式系统. 关于分布式系统, 主要了解如下基础知识
1. [CAP与BASE](/distributed_system/cap_base.md)
2. [延伸了解:一致性实现算法](/distributed_system/consistency.md)

## 什么是分布式事务

分布式事务就是指事务的参与者, 支持事务的服务器, 资源服务器以及事务管理器位于不同的分布式系统的不同节点之上, 分布式事务可以保证在一个事务内, 所有节点的操作要么全部成功, 要么全部失败, 从而保证数据一致性.

谨慎使用分布式事务, 要明确知道为什么要使用分布式事务.

一般而言, 由于性能的原因, 实际中更倾向于使用本地事务而非分布式事务. 此处尤其需要注意的是, 要避免过度设计, 如对于无需拆分的service(如规模没有大到需要拆分), 最好使用单机部署, 使用本地事务, 而非硬套设计模式, 引入额外的成本和开销.

## 产生原因

分布式事务保证在同一事务内, 多个节点操作数据库的事务性(ACID).

----
多个service节点

多service节点通常是随着 SOA/微服务 架构的大规模使用而产生的. 如对于电商系统, 优惠券/积分/结算 拆分为多个系统, 由不同的团队维护/部署, 但一次购物涉及到多个系统间互相调用, 根据SOA规范, 各系统又是独立部署的, 如此便需加入分布式事务.

[SOA相关知识参考](#SOA)

----
多个resource节点

同SOA, 当业务规模变大之后, 通常会针对区域分库或根据业务垂直分库分表, 部署在不同区域. 不再举例.

## 解决方案

分布式事务算法思想一般参考分布式系统中一致性算法的实现.

常用的算法思想有 PC二阶段提交算法, 事务消息(通过消息队列实现, 如rocketmq) 等.

分布式算法的实现要考虑
1. 超时策略: 避免失效事务无限制占用资源.
2. 幂等

可以参考一些现有的框架
- [事务消息-rocketmq](https://help.aliyun.com/document_detail/43348.html)
- [Seata](https://github.com/seata/seata) 是阿里开源的一款分布式事务框架.

### 二阶段提交算法

XA事务机制实现参考了2PC(2阶段提交)算法.

大致思想为
1. 第一阶段: 事务管理器通知相关的所有数据库节点执行预提交操作, 而后节点通知事务管理器自身操作是否可以提交, 而后节点阻塞进程, 等待第二阶段事务管理器的通知.
2. 第二阶段: 事务管理器根据所有节点的返回结果, 决定是全部提交还是全部回滚.

优点
- 实现简单: XA事务在大多数数据库已有实现(如mysql) 

缺点
- 单点问题: 过度依赖事务管理器.
- 性能较低: 2PC算法在执行过程中会阻塞进程, 浪费资源.
- 当在第二阶段中, 数据库节点出现异常(如没有收到事务管理器的通知, 或提交异常等等), 会造成数据不一致.

----
Mysql XA事务使用 流程如下
1. XA start 开始一个 XA 事务, 并设置为 active 状态
2. 执行事务内的sql
3. 对于 active 状态的XA事务, XA end 将该事务置为 IDLE 状态
4. 对于 IDLE 状态的XA事务
  - XA prepare 将事务置为 prepare 状态. XA recover 可以列出处于 prepare 状态的XA事务.
  - `XA commit xid one phase` 用于预备和提交事务. 此时 XA recover 不会列出该事务, 因为该事务已经终止.
5. 对于 prepare 状态的XA事务
  - XA commit 用于提交并终止事务
  - XA rollback 回滚并终止事务

```sql
XA start `xid`
--sql
xa end `xid`
xa prepare `xid`
xa commit `xid`
```

有如下注意点
1. xid 必须全局唯一
2. 启用xa事务时, 无法启用本地事务(transaction)

X/Open DTP 模型

### TCC

TCC

### rocketmq事务消息

## 参考
- [分布式事务的全面介绍-掘进-作者:咖啡拿铁](https://juejin.im/post/5b5a0bf9f265da0f6523913b)

### SOA
> 参考 [SOA和微服务架构的区别-何明璐的回答](https://www.zhihu.com/question/37808426/answer/93335393)

SOA, Service-Oriented Architecture, 面向服务的体系结构. 将业务的不同功能单元/服务通过服务之间良好的接口/契约联系起来. 接口是采用中立的方式进行定义的, 它应该独立于实现服务的硬件平台, 操作系统和编程语言. 这使得构建在各种各样的系统中的服务可以使用一种统一和通用的方式进行交互.

微服务是在 SOA 之后提出的理念, 微服务相当于 SOA + 业务系统更进一步的组件化和服务化, 即依照SOA拆分服务接口, 然后独立 开发/设计/运维 的服务. 

具体了解可以通过如下例子: 假设我们想要做一个可以将各国语言翻译成中文的程序, 我们有如下做法

----

为每个语言开发一个工具包, 调用 trans 方法即可将该语言转换为中文, 然后在主程序中引入各个语言包. 项目结构如下
````
- entry/main.go
- languages
  - english
  - japanese
  - ...

package english
- trans.go
````

这么写有如下缺点: (基础包代指 languages 下的各个语言包)
1. 耦合度高: 当基础包里的函数直接被外界调用时, 由于基础包不知道暴露的函数是否被外界使用, 所以不能更改任何已暴露的函数, 否则可能导致其他业务崩溃. 变量等同理.
2. 运维困难: 每次更改基础包时, 即使只更改一个, 也需要重新编译/部署整个项目. 且当某一功能存在问题时, 可能导致整个业务崩溃(如english包转换时出现问题, 导致无限占用资源, 就会导致整个进程逐渐卡死)
3. 扩展困难: 当单机无法满足业务量时, 如访问量增大, 只能通过增加节点解决.

---

SOA 解决了业务中耦合度的问题, 为之后服务拆分打下基础. 关于如何拆分业务, 可以了解下 业务领域模型.

引入SOA: 基础包定义服务接口, 服务保证接口实现的功能的稳定, 对外屏蔽服务的具体实现. 此时基础包接口如下
````
package english
- internal    // 在go中, 外界包访问不到internal下的内容
  - trans.go  // 定义具体实现
- trans.go    // 定义需要暴露的结构体和接口
````

---

各服务经过拆分后, 我们发现只要解决服务间通信的问题, 就可以通过单独部署各服务从而解决运维和扩展的问题.

刚开始, 企业一般使用 ESB 作为服务间通信工具. 因为我对ESB不太了解, 所以不再详述.

后来, 微服务的概念兴起, 各个服务通过 http/rpc/amqp 等方式互相通信, 服务通过docker部署等.

当然, 微服务使用的技术目前也不只这些, 以后也许会更多. 这里主要想说的是, 微服务的理念 = SOA + 进一步的组件化/服务化, 如上文中给的定义. 当然, 技术是在不断演进的, 后续关于此术语的描述可能不断变化, 但其思想是不变的, 如 最近很火的 面向接口编程, 不就是SOA思想么? 如微服务化, 也算是分布式系统的一种体现吧. 所以, 不要纠结于术语/定义, 要了解其思想. (就像设计模式一样, 需要求神不求形)

