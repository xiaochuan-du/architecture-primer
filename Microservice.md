# Reading list:

- [Microservice website](http://microservices.io/patterns/microservices.html)
- 推荐读物（Book：REST 实战）



# Reference list:

- Building Microservice 
- [best-practices-for-building-a-microservice-architecture](http://www.vinaysahni.com/best-practices-for-building-a-microservice-architecture)




# Microservice



## Definition

A **microservice** architecture shifts around complexity. Instead of a single complex system, you have a bunch of simple services with complex interactions.



### Key Benefits

- Technology Heterogeneity: 多种Tech Stack相互融合
- Resilience：分布式系统，分布风险但扩散问题
- Scaling：组件按需分配到资源
- Ease of Deployment：组件自己定义发布周期
- Organizational Alignment：与service boundary 相符合的团队构建
- Composability： 组件相互组合构成产品
- Optimizing for Replaceability： 组件升级不影响整体



### Relation with SOA

As a specific approach for SOA in the same way that XP or Scrum are specific approaches for Agile software development.



### Similar stacks (Language Specific)

- Shared Libraries
- Modules



### Microservice Should be

- **Maximize team autonomy**: Create an environment where teams can get more done without having to coordinate with other teams.
- **Optimize for development speed**: Hardware is cheap, people are not. Empower teams to build powerful services easily and quickly.
- **Focus on automation**: People make mistakes. More systems to operate also means more things that can go wrong. Automate everything.
- **Provide flexibility without compromising consistency**: Give teams the freedom to do what's right for their services, but have a set of standardized building blocks to keep things sane in the long run.
- **Built for resilience**: Systems can fail for a number of reasons. A distributed system introduces a whole set of new failure scenarios. Ensure measures are in place to minimize impact.
- **Simplified maintenance**: Instead of one codebase, you'll have many. Have guidelines and tools in place to ensure consistency.





## The Evolutionary Architect

### Zoning

：创造系统对开发者足够友好

工作范畴：

- 系统间交互
- 控制 Tech Stack
  - too little: fixed to one solution, different to improved by new tech
  - too much: out of control, developers hard to move from one project to the other
- 定义技术愿景：
  - 服从公司战略目标，制定技术路线
  - 制定原则：
    - 原则保证技术stack与公司目标一致，包括约束（原则自己定义、约束外界控制）
    - eg：目标：缩短新功能上线时间。原则：交付团队需要控制软件整个生命周期。
    - eg：目标：在其他国家团长业务。原则：在响应国家快速部署
    - [Heroku 12 Factors](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c)
  - 实践：
    - 实践保证原则得到落实，实践通常与技术相关
    - 常见实践包括：
      - 代码规范
      - 日志规范
      - HTTP／REST标准
    - eg：对于缩短上线时间的目标，实践是需要单独AWS account实现。使得资源自助管理和与其他团队隔离。
  - 原则与实践相结合，对于小团队可以是一套，对于大团队实践需要映射到原则。eg，原则对应 .Net实践和java实践。
- 治理：通过评估人员需求、当前情况、下一步的可能，来确保企业目标的达成。并且对于一致性的方向和目标进行监督。
- 方案取舍
- 花时间和团队一起工作，
- 制定方案不能让研发人员感觉到痛苦。
- 所有工作的方式
  - 架构师领导的小组讨论：
    - 出现异议：
      - 风险小，可以按照团队方案实施，在错误中学习
      - 风险高需要提前预防
      - ge，小孩子学骑自行车／过马路

方法：

- 定期跟团队一起工作，发现工作问题。
- Target -> Principle -> Practice 对应关系维护方式
  - 文档 
  - 实例代码
  - 工具 （推荐）

架构师职能小结：

- 愿景：设定、维护
- 同理心：考虑决定对于客户和同事的影响
- 合作：沟通产生结论
- 适应性：根据客户和组织的需求调整技术愿景
- 治理：确保技术愿景按照要求实现。

### The Required Standard

**好服务的特点:**

- 考虑到自治性
- 统筹全局


- 松耦合
- 高内聚



**Bounded Context**

每个Domain需要多个Bounded Content，一部分不需要外接通信，而另一部分需要与外界通信。

如果服务边界和上下文边界一致，并且为服务架构能够很好的表示这些边界，可以认为实现了高内聚低耦合的第一步。

在服务拆分之前，需要明确边界，如果边界不明确，可以先一起上线，等到边界明确后进行重构。切分服务不应该依靠技术话费，而应该考虑应用场景。



#### Monitoring

| Type | Name         | Usage  |
| ---- | ------------ | ------ |
| Tool | Nagios       | 检测     |
| Tool | Graphite/ELK | 收集指标数据 |
|      |              |        |

HealthCheck，CloudWatch，）



#### Interfaces

HTTP/REST

version，API

GraphQL



#### Architecture Safety

一组服务down了如能能不影响全局？

正确处理一下三种请求返回：

1. 正常、处理正确
2. 错误请求，服务器识别了错误（业务错误）
3. 服务器down了，没办法做出判断



### Governance Through Code

Ensure colleagues following w/o burden. 

- Exemplars
- Providing service templates







#### Platform

Dashboard:

- continuous integration
- monitoring
- logging 
- documentation: 
  - should be updated when services are updated.
  - The notification system should have knowledge of who the *current* owners are, accounting for any team or ownership changes.
- ​

Slack a custom bot:

-  triggering tests and deploys
-  requesting quick stats about a running service
-  chat logs also become an audit trail of past actions.

Independently Developed & Deployed

- be careful about shared libraries

Private Data Ownership: 

- Sharing data storage is a sneaky source of coupling. 
- Select the right database technology based on the use cases of the service.

Shared data server?

- a single point of failure 
- One service impacts another

Identifying Service Boundaries

- loosely coupled
- high cohesion
- each service responsible for it's own bounded context
- [Domain driven design](https://en.wikipedia.org/wiki/Domain-driven_design) ,
- Address sync operation to deal with sync scenarios,  eg, subscribe an event stream by two processors.

How big is microservice:

- A service should be *small enough* that it serves a focused purpose. 
- *big enough* that it minimizes interservice communication.
- Include but not limited to:
  - backend service
  - db
  - cache
  - job queues

Eventual Consistency

- some services have a divergent view of the underlying customer requests
- need to confirm if the lag is acceptable
- shields one service from failures in other services.

**idempotency**/ Idempotent operations

- easy to scale worker processes.
- Reduces error scenarios
- retrying jobs 

Load balancers:

- server side
- client side, choose which servers to go



Security:

**Layer your security**

-  Also known as [defence in depth](https://en.wikipedia.org/wiki/Defense_in_depth_(computing)).

- ​

  ​

1. ​



## Integration

### Protocol

-  SOAP
-  REST
-  XML-RPC
-  REST
-  Protocol Buffers

### Consideration

- 修改不应该影响消费者
- API 的技术无关性
- 易于消费者使用，但是不耦合特定消费者
- 隐藏内部细节

### Methods

##### 共享数据库

Cons：

- 暴露细节，一改全改
- 数据库技术升级
- 维护职责不明确。

##### Sync & Async

Async

- Request/ response: 设置回调
- 事件

Sync

- Request/ response

##### Orchestration（编排） & choreography（协同） 

Orchestration:

- 单一中心太多指责

Choregraphy：

- 一个实践多个订阅者
- 重量级的协同容易导致不稳定，修改代价比较大。需要跨服务监控。

**Request/ response：**

- RPC (SOAP, Thrift, protocol buffers)
  - 易于使用
  - 与特定语言绑定
  - 本地调用和远程调用不一致（网速、装载时间）
  - 代码修改需要考虑调用方
- REST
  - [steps toward the glory of REST](https://www.martinfowler.com/articles/richardsonMaturityModel.html)
  - 在客户端可以服务发现API而不是hardcode，回报需要长期才能体现出来
  - XML 可以利用XPATH查找元素，JSON轻量
  - 开发模式：先设计接口，等接口稳定之后再实现内部的持久化。在此期间，在硬盘上做持久化的工作。缺点是推迟了数据库部分的集成，对于新服务这样的设计是可以接受的。
  - 缺点：
    - 客户端解析参数代码和服务端无法公用。
    - 有些Web框架对于动词支持不全，PUT，PATCH
    - 不适合低延迟场景因为数据不够紧凑

**Event driven**

- 推荐读物：企业集成模式
- 消息代理（Rabbit MQ）尽量让消息处理中间件保持简单，逻辑在自己的服务中。
- HTTP传播事件

需要考虑

- 为服务发布实践机制
- 消费者接受机制

需要依靠死信队列，防止catastrophic failover。

短生命周期的操作易于管理，长生命周期的容易出现问题。



**服务即状态机**

如果出现了在客户服务之外与其进行相关修改的情况，那就失去的内聚性。

把关键领域的生命周期显式的建模出来非常有用

- 一个地方处理冲突
- 在状态变化基础上封装一些行为



Reactive extensions（Rx，响应式扩展）：组合起来多个调用的结果



DRY 和代码重构用

夸服务前提下可以拷贝代码，为了避免库耦合。服务间引入大量的耦合会产生比拷贝代码更多的问题。

需要考虑是否需要客户端代码

功能

- 服务发现
- 故障处理
- 日志打印



 按引用访问

- 客户端告诉服务端资源位置，服务端在需要时再做查询处理，降低从动作出发到结果产生的延时
- 服务端给客户端数据中带有有效时常，客户端在时长内做缓存处理。
- 客户端在处理来自服务端的数据时应考虑到时效性，谨慎处理



代码版本管理

系统设计Postel法制：

对自己的东西要求严格，对接受来的东西要宽容。这样的设计会在服务方发生变化时尽量少的修改代码。

版本号码

major.minor.patch

major向后不兼容的修改

minor新功能

patch修复bug

使用扩展收缩模式解决接口兼容性的问题，直到老版本的接口无人使用再停止响应版本的代码。

API version contro methods：

- HTTP Header
- /v1/api
- /api?version=1



如果两个版本API的过程过于漫长，需要考虑写新接口：

- 需要部署两次（新、旧对于hardcode ／v1/来说）
- 过程需要数据库同步，路由，中间件的保证
- API不同但是持久层保持一致


最好新老交替时间不长。



**User Interface**

组合API需要考虑服务与前端的组合，比如不同分辨率提供不同数据，组合API方法

- 前端调用多个API（API Gateway）
- UI片段组合（返回widget）


- Backends For Frontends(BFF) 每一个应用都有专著的后端，后端访问通用服务。

选择需要考虑高内聚，低耦合，抽象共同部分。



一个服务是购买还是自己组建？

如果通用服务就购买，如果是核心资产就自己研发。



集成第三方服务

- 定制化
- 意大利面集成
- 包装暴露新街口
- Strangler Appliatoin Pattern：route request from previous service to new services step by step



### Decompose monolithic system

接缝：抽取独立的代码不会影响系统其它部分，服务边界。

在一个地区人员对一种service完全负责。

减少外建使用可以减少服务化拆分时的设计负担。

重构数据库，分离不同服务的存储结构。

## 分布式事务

两阶段提交。

- 投票阶段：参与者告诉事务管理器是否需要继续。
- 一个否定票就回退。否则成功。（隐藏风险，因为最终投票也可能失败）


一致性事务尽量不要放到不同的微服务里面，如果一定需要要显式的创建一个概念来表示这一事务，目的是为了补偿方便。Eg，处理中的订单。

## 报告

- 通过服务调用来获取数据，通过操作批量数据的API，降低调用次数
- 数据导出：将数据从各个库分别导出，需要了解各个数据库的表结构。使用试图可以减少对表结构先验知识的依赖。S3作为核心存储。筛选后进入Tableau或者Excel。
- Transaction event 导出：修改数据的事件暴露给好多消费者，此方法耦合低，时效性高。数据量大的时候扩展难度较大。
- Netflix 基于Cassandra的备份机制，备份在s3中，使用Hadoop进行数据处理，Aegisthus。
- 实时性对报告的产生有影响。



## CI

CI 三个问题：

- 每天code check in 到build分支
- CI具有测试功能
- 构建失败是需要修改的第一要务

每一个微服务，有一个源代码库和CI build。持续集成服务器可以只有一个。

Pipeline

编译-》快速测试-》耗时测试-》用户验收测试（User Acceptance Testing）-》性能测试-》生产环境

定制化镜像的几种方法

- 默认镜像
- Puppet／Chef 应用版本控制的配置镜像
- 从实例烧制镜像（Packer工具提供本地和云的一致性烧录过程）

**配置漂移**：使用环境和配置好的不一致。

### 环境

tradeoff 类生成环境 and 快速反馈。约接近生产环境overhead越多，越接近开发环境反馈速度越快。

如果为了不同环境构建了不同的构建物

- 必须在构建物在使用前进行测试，common-dev测试结果在common-test是不能重用的。
- 构建比较耗时
- 构建的时候需要知道存在哪些环境
- 需要把敏感的配置数据放到代码里面。

**比较好的方法是一套构建物，配合不同的环境变量和配置文**件。

Host-service

- 一个主机一个服务
- 单主机多服务
- 单主机多服务容器
- 平台即为服务（Elastic Beanstalk）

自动化配置

Vagrant是个部署平台，在测试盒开发环境使用，



FollowUp

**一个Pipeline和多个环境如何结合？**

IAC和部署过程相结合。

自动化。



## 测试

**测试类型 Testing Diagram**



![](http://www.gallop.net/blog/wp-content/uploads/2016/03/blog-image.png)

Q1: xUnit

Q2: Fit-Finesse/ BDD/ Cucumber

Q3: Manually

Q4: load_http, etc



**测试范围 Testing pyramid** 

![](https://www.symbio.com/wp-content/uploads/2014/08/agile_pyramid2.jpg)



- 在单元测试阶段需要mock独立功能，所有运算需要在内存中完成。
- 集成测试中需要打桩服务，如虚拟数据，数据库内容建立，完成测试后打桩服务自动清理。

尽量使用小范围测试取代端到端测试

单元测试，开发团队自己负责。

集成测试夸团队共享代码权利，共同维护质量。

所有版本要进行单独的release。

（GUI Tests）端到端测试只覆盖少量的核心场景，不是覆盖全部的用户故事，全部细节需要在Unittest中体现。

**Consumer-Driven Contract**（消费者驱动的契约）：CDC根据消费的需求形成契约，变成CI的一部分，如果满足不了消费者的预期，那么服务无法上线。

Pact是CDC的工具，核心思想是消费者定义使用样例子，生产者满足消费者需求。

1. 消费者用Ruby DSL定义对生产者的期望。
2. 本地启动Mock服务器，生产Pact规范文件。JSON。
3. 生产者这边利用JSON Pact规范来驱动对于API的调用。

区分部署和上线：

- Blue／Green deployment，附属之后只有通过了冒烟测试，才release。
- Canary releasing 金丝雀发布：同时保持一定比例的新旧版本。新版本高于基线分数才作为正式release。

Mean Time Between Failures, MTBF：？？？

Mean Time To Repair, MTTR: 回滚，蓝绿部署

## 代码治理

把技术共识推行到工作中

### 范例

可以运行的代码范例



### 裁剪服务代码模版

工具

- Dropwizard
- Karyon

机遇开发语言的模版建立

考虑模版升级对每个工程的影响



## 技术债务

由于实践紧张、任务重技术债务需要补偿。

架构师需要有一个list列出需要偿还的债务。



## 集中治理和领导



## Exception Management

 需要违背原则的场景，eg 一般用Mysql 但是数据增长过快使用cassandra



## 团队建设

Grow teammates to take responsibility and face new challenges, so that they have changes to meet their career target. 

[Service discovery](https://www.youtube.com/watch?v=FGbzS6ripXA#t=10.573251)

[Microservice Introduction](http://microservices.io/patterns/microservices.html)

[Netflix microservice pattern](http://techblog.netflix.com/2013/01/optimizing-netflix-api.html)

