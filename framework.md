# Knowledge structure

## Theory

- System design topics: start here
  - [Step 1: Review the scalability video lecture](https://github.com/donnemartin/system-design-primer#step-1-review-the-scalability-video-lecture)
  - [Step 2: Review the scalability article](https://github.com/donnemartin/system-design-primer#step-2-review-the-scalability-article)
  - [Next steps](https://github.com/donnemartin/system-design-primer#next-steps)
- [Performance vs scalability](https://github.com/donnemartin/system-design-primer#performance-vs-scalability)
- [Latency vs throughput](https://github.com/donnemartin/system-design-primer#latency-vs-throughput)
- Availability vs consistency
  - CAP theorem
    - [CP - consistency and partition tolerance](https://github.com/donnemartin/system-design-primer#cp---consistency-and-partition-tolerance)
    - [AP - availability and partition tolerance](https://github.com/donnemartin/system-design-primer#ap---availability-and-partition-tolerance)
- Consistency patterns
  - [Weak consistency](https://github.com/donnemartin/system-design-primer#weak-consistency)
  - [Eventual consistency](https://github.com/donnemartin/system-design-primer#eventual-consistency)
  - [Strong consistency](https://github.com/donnemartin/system-design-primer#strong-consistency)
- Availability patterns
  - [Fail-over](https://github.com/donnemartin/system-design-primer#fail-over)
  - [Replication](https://github.com/donnemartin/system-design-primer#replication)

## Components

- [Domain name system](https://github.com/donnemartin/system-design-primer#domain-name-system)
- Content delivery network
  - [Push CDNs](https://github.com/donnemartin/system-design-primer#push-cdns)
  - [Pull CDNs](https://github.com/donnemartin/system-design-primer#pull-cdns)
- Load balancer
  - [Active-passive](https://github.com/donnemartin/system-design-primer#active-passive)
  - [Active-active](https://github.com/donnemartin/system-design-primer#active-active)
  - [Layer 4 load balancing](https://github.com/donnemartin/system-design-primer#layer-4-load-balancing)
  - [Layer 7 load balancing](https://github.com/donnemartin/system-design-primer#layer-7-load-balancing)
  - [Horizontal scaling](https://github.com/donnemartin/system-design-primer#horizontal-scaling)
- Reverse proxy (web server)
  - [Load balancer vs reverse proxy](https://github.com/donnemartin/system-design-primer#load-balancer-vs-reverse-proxy)
- Application layer
  - [Microservices](https://github.com/donnemartin/system-design-primer#microservices)
  - [Service discovery](https://github.com/donnemartin/system-design-primer#service-discovery)

- Asynchronism
  - [Message queues](https://github.com/donnemartin/system-design-primer#message-queues)
  - [Task queues](https://github.com/donnemartin/system-design-primer#task-queues)
  - [Back pressure](https://github.com/donnemartin/system-design-primer#back-pressure)
- Communication
  - [Transmission control protocol (TCP)](https://github.com/donnemartin/system-design-primer#transmission-control-protocol-tcp)
  - [User datagram protocol (UDP)](https://github.com/donnemartin/system-design-primer#user-datagram-protocol-udp)
  - [Remote procedure call (RPC)](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc)
  - [Representational state transfer (REST)](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest)

## Database

- Relational database management system (RDBMS)
  - [Master-slave replication](https://github.com/donnemartin/system-design-primer#master-slave-replication)
  - [Master-master replication](https://github.com/donnemartin/system-design-primer#master-master-replication)
  - [Federation](https://github.com/donnemartin/system-design-primer#federation)
  - [Sharding](https://github.com/donnemartin/system-design-primer#sharding)
  - [Denormalization](https://github.com/donnemartin/system-design-primer#denormalization)
  - [SQL tuning](https://github.com/donnemartin/system-design-primer#sql-tuning)
- NoSQL
  - [Key-value store](https://github.com/donnemartin/system-design-primer#key-value-store)
  - [Document store](https://github.com/donnemartin/system-design-primer#document-store)
  - [Wide column store](https://github.com/donnemartin/system-design-primer#wide-column-store)
  - [Graph Database](https://github.com/donnemartin/system-design-primer#graph-database)
- [SQL or NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql)

## Cache

- [Client caching](https://github.com/donnemartin/system-design-primer#client-caching)
- [CDN caching](https://github.com/donnemartin/system-design-primer#cdn-caching)
- [Web server caching](https://github.com/donnemartin/system-design-primer#web-server-caching)
- [Database caching](https://github.com/donnemartin/system-design-primer#database-caching)
- [Application caching](https://github.com/donnemartin/system-design-primer#application-caching)
- [Caching at the database query level](https://github.com/donnemartin/system-design-primer#caching-at-the-database-query-level)
- [Caching at the object level](https://github.com/donnemartin/system-design-primer#caching-at-the-object-level)
- When to update the cache
  - [Cache-aside](https://github.com/donnemartin/system-design-primer#cache-aside)
  - [Write-through](https://github.com/donnemartin/system-design-primer#write-through)
  - [Write-behind (write-back)](https://github.com/donnemartin/system-design-primer#write-behind-write-back)
  - [Refresh-ahead](https://github.com/donnemartin/system-design-primer#refresh-ahead)


- ​
- [Security](https://github.com/donnemartin/system-design-primer#security)



## Appendix

- [Powers of two table](https://github.com/donnemartin/system-design-primer#powers-of-two-table)
- [Latency numbers every programmer should know](https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know)
- [Additional system design interview questions](https://github.com/donnemartin/system-design-primer#additional-system-design-interview-questions)
- [Real world architectures](https://github.com/donnemartin/system-design-primer#real-world-architectures)
- [Company architectures](https://github.com/donnemartin/system-design-primer#company-architectures)
- [Company engineering blogs](https://github.com/donnemartin/system-design-primer#company-engineering-blogs)
- [Under development](https://github.com/donnemartin/system-design-primer#under-development)
- [Credits](https://github.com/donnemartin/system-design-primer#credits)
- [Contact info](https://github.com/donnemartin/system-design-primer#contact-info)
- [License](https://github.com/donnemartin/system-design-primer#license)



## Github Projects

[The most forked projects](https://github.com/search?o=desc&q=stars:%3E1&s=forks&type=Repositories)

[The most starred projects](https://github.com/search?q=stars:%3E1&s=stars&type=Repositories)



## Relationship

**Microservice** improved **Continuous Delivery/Deployment** Support (Easy update and rollback)

