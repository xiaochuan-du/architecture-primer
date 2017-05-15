# Microservice

Reading list:

- [Microservice website](http://microservices.io/patterns/microservices.html)
- 推荐读物（Book：REST 实战）

Reference list:

- Microservice 

演化式架构师：

工作职能：创造系统对开发者足够友好

职能：

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

  ​

## 定义一个好的服务：

- 考虑到自治性
- 统筹全局


- 松耦合
- 高内聚



### Bounded Context 

每个Domain需要多个Bounded Content，一部分不需要外接通信，而另一部分需要与外界通信。

如果服务边界和上下文边界一致，并且为服务架构能够很好的表示这些边界，可以认为实现了高内聚低耦合的第一步。

在服务拆分之前，需要明确边界，如果边界不明确，可以先一起上线，等到边界明确后进行重构。切分服务不应该依靠技术话费，而应该考虑应用场景。



#### 监控

工具

- 检测 Nagios
- Graphite 收集指标数据
- 日志收集 （HealthCheck，CloudWatch，）

#### 接口

HTTP/REST

version，API

#### 架构安全性

一组服务down了如能能不影响全局？

正确处理一下三种请求返回：

1. 正常、处理正确
2. 错误请求，服务器识别了错误（业务错误）
3. 服务器down了，没办法做出判断



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

分布式事务，两阶段提交。

- 投票阶段：参与者告诉事务管理器是否需要继续。
- 一个否定票就回退。否则成功。（隐藏风险，因为最终投票也可能失败）



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



## What is Monolithic service pattern?

[Monolithic Introduction](http://microservices.io/patterns/monolithic.html)



## Why microservice?

- ##### Smaller Application Footprint (per Service)

Each separate service will inevitably have a smaller footprint which brings us the following benefits:

- ##### Comprehensibility

Smaller applications are generally easier to understand, debug and change than complex, large systems.

- ##### Shorter Development Time

Smaller applications start-up time is shorter, making developers more efficient in their dev-test cycles and allowing for agile development.

- ##### Improved Continuous Delivery/Deployment Support

Updating a large complex system takes a lot of time while the implication of each change is hard to identify. On the other hand – when working with microservices – each service is (or should be) deployable independently, which makes it much easier to update just a part of the system and rollback only the breaking change if anything goes wrong.

- ##### Scalability Improvement

When each service is scalable independently it becomes easier to provision the resources adapted for its special needs. Additionally we don’t need to scale everything. We can for example run a few instances of front-end logic but only one instance of business logic.

- ##### Easier to adopt new frameworks/technologies.

Unlike a monolith we don’t have to write everything in Java or Python (for example). As each service is installed and built independently – it can be written in a different language and based on a different framework. As long as it supports the well-defined API/message protocol to communicate with other services.

- ##### Allows for a more efficient organizational structure.

It’s been noted by a [number of studies](http://www.qsm.com/process_improvement_01.html) that individual performance is reduced when the overall team size grows beyond a specific size (namely 5-7 engineers). (See also the [2-Pizza-Team](http://blog.idonethis.com/two-pizza-team/) concept) This is caused by several reasons like coordination/communication complexity, the ‘[social loafing](https://en.wikipedia.org/wiki/Social_loafing)’ and ‘free rider’ phenomena. Microservice architecture allows a small team to maintain the development of each service – thus allowing for more efficient development iterations and improved communication.

And now that we’ve outlined the benefits, let’s look at the challenges of this architectural pattern as manifested in CI/CD concerns.



### The Challenges of Microservice Architecture (from CI/CD perspective)

All challenges of microservices are caused by the complexity which originates from the fact that we are dealing with **a distributed system**. Distributed systems are inherently more difficult to plan, build and reason about. Here’s a list of specific challenges we will encounter:

- **Dependency Management**

One of the biggest enemies of distributed architectures are dependencies. In a microservice-based architecture, services are modeled as isolated units that manage a reduced set of problems. However, fully functional systems rely on the **cooperation and integration** of its parts, and microservice architectures are not an exception. The question then becomes: how do we manage dependencies between multiple fast-moving, independently evolving components?

- **Versioning and Backward Compatibility**

Microservices are developed in isolation with each service having its own distinct lifecycle. This requires us to define very specific versioning rules and requirements. Each service has to make absolutely clear which versions of dependent services it relies upon.

- **Data partitioning and sharing**

Best practices of microservice development propose having a separate database for each service. However this isn’t always feasible and surely is never easy when you have transactions spanning multiple services. In CI/CD context this may mean we have to deal with multiple **inter-related schema updates**.

- **Testing**

While being able to operate in isolation – a microservice isn’t worth much without its counterparts. On the other hand – bringing up the full system topology for testing just one service cancels out the benefits of modularity and encapsulation that microservices are supposed to bring. The challenge here is to **define the minimum viable system testing subset and/or provide good enough mockups/stubs for testing in absence of real services**. Additional challenges lie in service communication patterns which mostly occur over network and therefore must take in account possible network hiccups and outages.

- **Resource Provisioning and Deployment**

In a microservice architecture each service should be independently deployable. On the other hand we need a way to know where and how to find this service. We need a way to tell our services where the shared resources (like databases, data collectors and message queues) reside and how to access them. This brings about the need for **service discovery**, configuration separation from business functionality and failure resilience in case certain service is temporarily missing/unavailable.

- **Variety/Polyglossia**

Microservices allow us to develop each service in a different language and using a different framework. This lets us use the right tool for the job in each individual case but it’s a mixed blessing from the delivery viewpoint. Because our delivery pipeline is most efficient when it defines a clear unified framework with distinct building blocks and a simple API. This may become challenging when having to support a variety of technologies, build tools and frameworks.

### Tackling the Microservice Architecture Challenges in the CI/CD Pipeline (and beyond)

Now that we’ve outlined the challenges accompanying the delivery of microservice-based systems, let’s define the best practices for dealing with them when building a modern CI/CD pipeline.

### **Dependency Management:**

Even before looking at build-time dependency management we need to look at the wider concepts of service inter-dependency. With microservices each service is meant to be able to operate on its own. Therefore in an ideal setting no direct build-time dependencies should be needed at all. At the maximum a dependency on a common communication protocol and API version can be in place, with version changes taken care of by backward compatibility and consumer-driven contracts.

In order to achieve this the architectural concepts of **loose coupling** and **problem locality** should be applied when splitting up our system into separate services.

- **Loose coupling**: microservices should be able to be modified without requiring changes in other microservices.
- **Problem locality**: related problems should be grouped together (in other words, if a change requires an update in another part of the system, those parts should be close to each other).



- - - If two or more services are too tightly coupled – i.e. have too many correlated changes which require careful coordination – it should be considered to unify them into one service.
    - If we’re not in the ideal setting of loose coupling and concern separation and re-architecting the system is currently impossible (for lack of resources or business reasons) then strict [semantic versioning](http://semver.org/spec/v2.0.0.html) should be applied to all interdependent services. This is to make sure we are building and deploying against correct versions of service counterparts.

### **Versioning:**

As stated in the previous paragraph – semantic versioning is a good way of signifying when a breaking change occurs in the service semantics or data schema. In practice this means that any given service should be able to talk to another service as long as the contract between them is sealed. With the MAJOR field of semantic version being the guarantee of that seal. For experimental or feature branches – the name of the branch can be added as metadata to the version name as suggested here: http://semver.org/#spec-item-10

### **Data Partitioning:**

- If each service is based on its own database then database schema changes and deployment becomes the responsibility of that service installation scripts. For the CI/CD pipeline it means we need to be able to support multiple database deployment in our test and staging environment provisioning cycles.


- If services share databases it means we need to identify all the data dependencies and retest all the dependent services whenever a shared data schema is changed.  

### Testing

- For a much deeper look at microservice testing patterns look here: [http://martinfowler.com/articles/microservice-testing](http://martinfowler.com/articles/microservice-testing)
- **Deployment **of individual services should be a part of the end-to-end test to verify successful upgrade and rollback procedures as part of the full system test.
- **End-to-End Tests **should be only executed after unit and integration tests have completed successfully and test coverage thresholds have been met. This is because the setup and execution of the e2e environment tends to be difficult and error-prone and we should introduce sufficient gating to ensure its stability.
- In such a case it may be a good idea to separate **integration tests** in CI pipeline so that external outages don’t block development.
- Due to interservice communication reliance on network and overall system complexity integration tests can be expected to fail with higher frequency due to non-related infrastructure or version dependency errors.
- **Integration tests**: As stated earlier – the minimum viable subset of interdependent services should be identified wherever possible to simplify testing environment provisioning and deployment.
- **Automated Deployment **becomes absolutely necessary with each service deployable by itself and a deployment orchestration solution (e.g Ansible playbook) describing the various topologies and event sequences.
- **Test doubles **(Mocks, Stubs, etc) should be encouraged as a tool for testing service functionality in isolation.
- **Coverage thresholds** are a good strategy for ensuring we’re writing tests for all the new functionality.
- **Unit tests** become especially important in microservice environment as they allow for faster feedback without the need to instantiate the collaborating services. [Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development) should be encouraged and unit test coverage should be measured.

### **Resource Provisioning and Deployment**

- Infrastructure-as-Code approach should be used for versioned and testable provisioning and deployment processes.
- Microservices should enable horizontal scaling across a compute resource cluster. This calls for using:
  - A central configuration mechanism in a form of a distributed Key-Value store (such as Consul or etcd). Our CI/CD pipeline should support separate deployment of configuration to that store.
  - A cluster task scheduler (e.g Docker Swarm, Mesos, Kubernetes or Nomad). The CD process needs to interface with whichever system we choose  for implementing scratch rollouts, rolling updates and [blue/green deployments.](http://searchitoperations.techtarget.com/definition/blue-green-deployment)
  - Microservice architectures are often enabled by OS container technologies like Docker. Containers as a packaging an delivery mechanism should definitely be evaluated.

### **Variety/Polyglossia**

It is very desirable to base the CI/CD flow for each service on the same template which includes the same building blocks and a well defined interface. I.e each service should provide similar ‘build’ , ‘test’, ‘deploy’ and ‘promote’ endpoints for integration into the CI system. Additionally the interface for querying service interdependency should be clearly defined. This will allow for almost instant CI/CD flow creation for each new service and will reduce the load on the CI/CD team. Ultimately this should allow developers to plug-n-play new services into the CI/CD system in a fully self-service mode.



[Service discovery](https://www.youtube.com/watch?v=FGbzS6ripXA#t=10.573251)

[Microservice Introduction](http://microservices.io/patterns/microservices.html)



[Netflix microservice pattern](http://techblog.netflix.com/2013/01/optimizing-netflix-api.html)



## Tools

| Tools                                | Function                                 | Cons                         | Pros                                     |      |
| ------------------------------------ | ---------------------------------------- | ---------------------------- | ---------------------------------------- | ---- |
| ECS                                  | Orchestration                            | lose request during updating | Support AWS ECS natively                 |      |
| ECR                                  | Registry                                 |                              |                                          |      |
| [Consul](https://www.consul.io/)     | Service Discovery  <br> Failure Detection <br> Multi Datacenter <br> KV Storage |                              |                                          |      |
| [Kubernetes](https://kubernetes.io/) | Orchestration <br> Service Discovery     | Service Discovery            | Run Anywhere <br> Planet Scale <br> Never Outgrow <br> Uncouple with cloud |      |
| OpenShift Origin                     |                                          |                              |                                          |      |



ECS does not provide a service discovery service, thus it needs to work with consul especially in China region. While, kubernetes seem to be able to collaborate well with AWS products, need more investigation on this topic. 



For dev only,

docker-compose up each containers







Kuberetes

**The Master is responsible for managing the cluster.** 

- scheduling applications
- maintaining applications' desired state
- scaling applications
- rolling out new updates.

Master provides **Kubernetes API** ,. 



A node is a VM or a physical computer that serves as a worker machine in a Kubernetes cluster.**

Kubelet: client, install Docker, A Kubernetes cluster that handles production traffic should have a minimum of **three** nodes.



**Minikube** is a lightweight Kubernetes implementation that creates a VM on your local machine and deploys a simple cluster containing only one node. 



**kubectl**: Commend line interface. 

- **kubectl get** - list resources

- **kubectl describe** - show detailed information about a resource

- **kubectl logs** - print the logs from a container in a pod

- **kubectl exec** - execute a command on a container in a pod

  ​

Kubernetes dashboard allows you to view your applications in a UI.

**Pod** is a Kubernetes abstraction that represents a group of one or more application containers (such as Docker or rkt), and some shared resources for those containers. Those resources include:

- Shared storage, as Volumes
- Networking, as a unique cluster IP address
- Information about how to run each container, such as the container image version or specific ports to use

The containers in a Pod share an IP Address and port space, are always co-located and co-scheduled, and run in a shared context on the same Node.



![pods](https://kubernetes.io/docs/tutorials/kubernetes-basics/public/images/module_03_pods.svg)



A Pod always runs on a Node. A **Node** is a worker machine in Kubernetes and may be either a virtual or a physical machine, depending on the cluster.

 Node can have multiple pods, and the Kubernetes master automatically handles scheduling the pods across the Nodes in the cluster. The Master's automatic scheduling takes into account the available resources on each Node.

Every Kubernetes Node runs at least:

- Kubelet, a process responsible for communication between the Kubernetes Master and the Nodes; it manages the Pods and the containers running on a machine.
- A container runtime (like Docker, rkt) responsible for pulling the container image from a registry, unpacking the container, and running the application.

*Containers should only be scheduled together in a single Pod if they are tightly coupled and need to share resources such as disk.*

![Node overview](https://kubernetes.io/docs/tutorials/kubernetes-basics/public/images/module_03_nodes.svg)



A **Service** in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them. Services enable a loose coupling between dependent Pods. 



[best practice](https://techbeacon.com/one-year-using-kubernetes-production-lessons-learned) 

[Write containers' log into cloudwatch](https://docs.docker.com/engine/admin/logging/awslogs/#credentials)

http://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_awslogs.html 

