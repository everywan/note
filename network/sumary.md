1. IANA

## 网关/DNS
网关: 不同 子网/网段 之间通信的中间节点.
- 实现两个网段之间的通信, 必须通过网关通信

网关和DNS区别
- DNS是用来将域名解析成IP地址, 而网关是用来找到该IP在哪个子网/网段.
- 比如说, 机器A请求 www.baidu.com, DNS 解析成IP地址 220.181.112.244, 但是该IP地址并不在机器A所在的网段中. 所以OS将TCP包扔给网关, 由网关去寻找该IP所在的网段. 其中, 机器A如何寻找网关的地址, 网关又如何寻找该IP地址所在的网段S, 网关又如何与S通信, 都是有统一的标准实现
- 再比如说, 机器A请求 www.localhost.com, DNS解析到本地(这句存疑, 还没有了解怎么解析到本地的), 然后与网关匹配, 发现属于同一子网段, 那么机器A通过ARP获取对方mac, 直接通信(存疑, 待验证)

## CDN
cdn: 内容分发网络(Content Delivery Network) 将资源站资源缓存至速度较快的服务器节点, 当用户访问/下载资源时, 无需请求源服务器.

cdn 接入: DNS解析域名时, 返回 IP 或 CNAME(cdn的域名), 然后客户请求 ip/cdn.
1. 当用户请求的资源在cdn上没有/过期时, 触发回源请求, cdn请求源服务器, 加载资源
2. 当cdn上有相关资源时, 直接返回.

CNAME: 真实名称记录, 用于将一个域名映射到另一个域名(真实名称, CNAME即该域名).

![图示](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/5098/15382038524886_zh-CN.png)
1. 终端用户（北京） 向 www.a.com下的某资源发起请求，会先向 LDNS 发起域名解析请求。
2. 当 LDNS 解析 www.a.com 时，会发现已经配置了 CNAME www.a.tbcdn.com。
3. 解析请求会发送至阿里云DNS调度系统，并为请求分配最佳节点 IP。
4. LDNS 获取 DNS 返回的解析 IP。
5. 用户获取解析 IP。
6. 用户向获取的 IP 发起对该资源的访问请求。
7. 若该 IP 对应的节点已经缓存了该资源，则会将数据直接返回给用户（如图中步骤7、8），此时请求结束。
8. 若该节点未缓存该资源，则节点会向业务源站发起对该资源的请求。获取资源后，结合用户自定义配置的缓存策略（可参考产品文档中的缓存配置），将资源缓存至节点（如图：北京节点），并返回给用户，此时请求结束。
