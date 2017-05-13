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

Scale out via the process model

[IX. Disposability](https://12factor.net/disposability)

Maximize robustness with fast startup and graceful shutdown

[X. Dev/prod parity](https://12factor.net/dev-prod-parity)

Keep development, staging, and production as similar as possible

[XI. Logs](https://12factor.net/logs)

Treat logs as event streams

[XII. Admin processes](https://12factor.net/admin-processes)

Run admin/management tasks as one-off processes