# Microservice

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

