# Introduction

In the modern era, software is commonly delivered as a service: called *web apps*, or *software-as-a-service*. The twelve-factor app is a methodology for building software-as-a-service apps that:

- Use **declarative** formats for setup automation, to minimize time and cost for new developers joining the project;
- Have a **clean contract** with the underlying operating system, offering **maximum portability** between execution environments;
- Are suitable for **deployment** on modern **cloud platforms**, obviating the need for servers and systems administration;
- **Minimize divergence** between development and production, enabling **continuous deployment** for maximum agility;
- And can **scale up**(X axis) without significant changes to tooling, architecture, or development practices.

The twelve-factor methodology can be applied to apps written in any programming language, and which use any combination of backing services (database, queue, memory cache, etc).

Recommended Books:

- *Patterns of Enterprise Application Architecture* 
- *Refactoring*.

# The Twelve Factors

[I. Codebase](https://12factor.net/codebase)

One codebase tracked in revision control, many deploys

[II. Dependencies](https://12factor.net/dependencies)

Explicitly declare and isolate dependencies

[III. Config](https://12factor.net/config)

Store config in the environment. Not in several config files, config parameters are stored in environment independently. 

[IV. Backing services](https://12factor.net/backing-services)

Treat backing services as attached resources

[V. Build, release, run](https://12factor.net/build-release-run)

Strictly separate build and run stages

[VI. Processes](https://12factor.net/processes)

Execute the app as one or more stateless processes. (No stick sessions)

[VII. Port binding](https://12factor.net/port-binding)

Export services via port binding

[VIII. Concurrency](https://12factor.net/concurrency)

Scale out via the process model?? No sure what is the meaning of this section.

The [share-nothing, horizontally partitionable nature of twelve-factor app processes](https://12factor.net/processes) means that adding more concurrency is a simple and reliable operation. The array of process types and number of processes of each type is known as the *process formation*.

Twelve-factor app processes [should never daemonize](http://dustin.github.com/2010/02/28/running-processes.html) or write PID files. Instead, rely on the operating system’s process manager (such as [Upstart](http://upstart.ubuntu.com/), a distributed process manager on a cloud platform, or a tool like [Foreman](http://blog.daviddollar.org/2011/05/06/introducing-foreman.html) in development) to manage [output streams](https://12factor.net/logs), respond to crashed processes, and handle user-initiated restarts and shutdowns.

[IX. Disposability](https://12factor.net/disposability)

Maximize robustness with fast startup and graceful shutdown

[X. Dev/prod parity](https://12factor.net/dev-prod-parity)

Keep development, staging, and production as similar as possible.



Gaps:

- The time gap: code takes days, months or years to production
- The personannel gap: , Developers write code, ops deploy it.
- The tools gap: Dev Nginx, SQlite and OS X, while prod deploys uses apache, Mysql and linux.
  - Differences between backing services mean that tiny **incompatibilities** crop up, causing code that worked and passed tests in development or staging to fail in production. These types of errors create friction that disincentivizes continuous deployment. The cost of this friction and the subsequent dampening of continuous deployment is extremely high when considered in aggregate over the lifetime of an application.

Solutions:

- CD: deploy continuously with meaningful functions.
- You build it, you deploy it.
- tools as similar as possible.
  - declarative provisioning tools such as [Chef](http://www.opscode.com/chef/) and [Puppet](http://docs.puppetlabs.com/) combined with light-weight virtual environments such as [Vagrant](http://vagrantup.com/) allow developers to run local environments which closely approximate production environments.

Takeaways:

- Dev/ Test: similar tech stack as prod via docker containers/ low-configed instances.
- Be carefull of version and type of each backing services.

[XI. Logs](https://12factor.net/logs)

Treat logs as event streams

**A twelve-factor app never concerns itself with routing or storage of its output stream.** It should not attempt to write to or manage logfiles. Instead, each running process writes its event stream, unbuffered, to `stdout`. During local development, the developer will view this stream in the foreground of their terminal to observe the app’s behavior.

**A twelve-factor app never concerns itself with routing or storage of its output stream.** It should not attempt to write to or manage logfiles. Instead, each running process writes its event stream, unbuffered, to `stdout`. 

- Dev: the developer will view this stream in the foreground of their terminal to observe the app’s behavior.
- Staging/ Prod:  each process’ stream will be captured by the execution environment, collated together with all other streams from the app, and routed to one or more final destinations for viewing and long-term archival. These archival destinations are not visible to or configurable by the app, and instead are completely managed by the execution environment. 

The event stream for an app can be routed to a file, or watched via realtime tail in a terminal.

Log routersToos: Open-source  (such as [Logplex](https://github.com/heroku/logplex) and [Fluent](https://github.com/fluent/fluentd)) are available for this purpose. Docker cloudwatch driver.

Log analysis : 

- Splunk
- Hadoop/ Hive

so that:

- Finding a event
- Large-scale graphing of trends (requests per minutes)
- Active alerting according to user-defined heuristics.



[XII. Admin processes](https://12factor.net/admin-processes)

Run admin/management tasks as one-off processes ???