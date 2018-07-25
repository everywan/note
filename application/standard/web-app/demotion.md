## 服务的开关,降级和熔断

### 开关
开关是指业务流程中那些非必要的接口调用可以随时被控制开关.

如 电商中, 在正常的交易下单环节, 需要调用 接口A: 获取商品信息, 接口B: 创建订单, 接口C: 相似商品推荐 三个接口, 但是在电商大促时, 服务器资源紧张, 由于C接口是非必要的, 所以在系统繁忙时可以通过管理界面/管理命令关闭C接口的调用, 等业务正常了再开启. 这种开关就叫 服务降级的开关

### 降级
服务降级是指 当服务器压力剧增的情况下, 根据当前业务情况及流量对一些服务和页面有策略的降级, 以此释放服务器资源以保证核心任务的正常运行.

服务降级方式有以下几种
1. 服务接口拒绝服务: 无用户特定信息, 页面能访问, 但是添加删除提示服务器繁忙. 页面内容也可在Varnish或CDN内获取.
2. 页面拒绝服务: 页面提示由于服务繁忙此服务暂停. 跳转到varnish或nginx的一个静态页面.
3. 延迟持久层: 页面访问照常, 但是涉及记录变更, 会提示稍晚能看到结果, 将数据记录到异步队列或log, 等待服务恢复后执行.
4. 随机拒绝服务: 服务接口随机拒绝服务, 让用户重试, 因为用户体验不佳, 目前较少有人采用.

#### 持久层降级方式

数据操作    |cache工作  |异步队列工作|
|:--------|:----------|:---------|
增        |禁止         |允许但不能重复|
删        |禁止         |允许但不能复合|
改        |禁止         |允许只保留结果|
查        |允许, 查询未命中则允许查询mysql/其他持久层|走cache|

#### 降级方式
1. 直接管理: 运维人员指定那些模块降级
2. 分级管理: 程序自动检测, 发送消息给运维人员, 运维人员根据消息选择降级等级.

#### 服务降低埋点
1. 消息中间件: 所有API访问都是用消息中间件控制
2. 前端页面: 指定页面不可访问
3. 底层数据驱动: 只允许查询, 不允许增删改.

### 熔断
服务熔断一般是指软件系统中, 由于某些原因使得服务出现了过载现象, 为防止造成整个系统故障, 从而采用的一种保护措施, 所以很多地方把熔断亦称为过载保护. 与降级不同的时, 熔断是由于某个服务访问频率过高, 而降级是整体系统过载.

比如在股票市场, 熔断是指当股指波幅达到某个点后, 交易所为控制风险采取的暂停交易措施. 比如 生活中插座上的 保险丝.