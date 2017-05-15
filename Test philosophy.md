# Test Philosophy

> Google’s Philosophy and Core Principles

> 1. Focus on the user and all else will follow.
> 2. It’s best to do one thing really, really well.
> 3. Fast is better than slow.
> 4. Democracy on the web works.
> 5. You don’t need to be at your desk to need an answer.
> 6. You can make money without doing evil.
> 7. There’s always more information out there.
> 8. The need for information crosses all borders.
> 9. You can be serious without a suit.
> 10. Great just isn’t good enough.

## Automated Testing



#### UI Test (End-to-End Test): 

- Mimic user interactions in the form of scripts.
- Run in form of a test.
- Acts just like a user.

Test a complete system end-2-end

Feature:

- Slow - they take seconds to run
- Fragile: they often fail by numerous reasons
- Not scalable - it is impossible to pass through all paths in any application but the very smallest
- Great for verifying that the most important flow through the application works

Thus, User more sparingly. Used for really important end-to-end style smoke tests



> In google, every night:
>
> 1. The latest version of the service is built. 
> 2. This version is then deployed to the team's testing environment. 
> 3. All end-to-end tests then run against this testing environment. 
> 4. An email report summarizing the test results is sent to the team.

#### Integration Test:

- Mimic the http calls
- Compared with UI test, it tests similar functionality in a slightly different way.

Feature:

- slow - they take seconds to run
- brittle - there are many reasons to why they fail
- not scalable - it is impossible to pass through all paths in any application but the smallest

Great for verifying that **important components are properly connected**

Used for Web services, tell us there are problems but can not tell us where



#### Unit test:

Focus on what the unit of code is doing, not what its dependencies do.

- fast - they take milliseconds to run
- stable - there is only one reason to why they fail
- scalable - it is possible to pass through all paths in any application
- Isolates Failures
- great for verifying that implementations you later depend on actually work

Feature:

- Highly precise
- Highly efficient
- Miss bugs that can sometimes only occur when you hook things up

Rely on the unit test for the **bulk** of our automating testing.



Everything it touches should be done in memory; this means that the test code **and** the code under test shouldn't:

- Call out into (non-trivial) collaborators
- Access the network
- Hit a database
- Use the file system
- Spin up a thread



#### The True Value of Tests 

If we "focus on the user (and all else will follow)," we have to ask ourselves how a failing test benefits the user. Here is the answer:

| Stage       | Failing Test | Bug Opened | Bug Fixed |
| ----------- | ------------ | ---------- | --------- |
| Value Added | No           | No         | Yes       |

Thus, to evaluate any testing strategy, you cannot just evaluate how it finds bugs. You also must evaluate how it **enables developers to fix (and even prevent) bugs**.



Bad testing design leads to :

- Higher maintenance costs
- Duplicate effort
- Pure waste



### Test pyramid

- Common language in teams
- Good rules of thumb, where and when to use different kinds of tests
- Save time and effort

![img](http://www.agilenutshell.com/assets/episodes/way-of-web/rules-of-thumb.png)

Google often suggests a 70/20/10 split: 70% unit tests, 20% integration tests, and 10% end-to-end tests.



### Automated Test for Microservice

- Contract Testing: A very good practice in micro-service architect.There are some solution for it. One of the most suitable one is a consumer-driven contract testing called "[pact](https://github.com/realestate-com-au/pact)".
- If possible, keep your Test Automation in the same repo since you may need Mocking/Stubing plus versioning can be tricky. Also staying in the same repo **could** be a help for you CI/CD pipeline.
- Have a look at Microservice testing by [M.Fowler](http://martinfowler.com/articles/microservice-testing/). It gives good idea about different layers of Micro service testing.



Testing in isolation

Testing in production

[consumer-driven contract process](https://martinfowler.com/articles/consumerDrivenContracts.html)

pact





### Code coverage

[Python coverage tutorial](http://www.tuicool.com/articles/RbEby2a)