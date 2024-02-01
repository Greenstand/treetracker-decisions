# Using a simple message queue + moderately coupled microservices to streamline our platform

* Status: proposed 
<!-- {proposed | rejected | accepted | deprecated | … | superseded by [ADR-0005](0005-example.md)} --> <!-- optional -->
* Deciders: @dadiorchen @Kpoke @ZavenArra @sebastiangaertner @arunbakt 
<!-- Find deciders here: https://github.com/orgs/Greenstand/people  -->
* Date: Thu Feb  1 05:58:58 CST 2024

Technical Story: {description | ticket/issue URL} <!-- optional -->

## Context and Problem Statement

### Problem

I believe for now the strict coupling between the microservices and the utilization of rabbitmq blocks us from pushing forward the designed new domain target and all those new features like capture+tree binding.

Currently, we strictly limit the microservices to having access to database schema and use rabbitmq to decouple the microservices. This makes the process of our business code indirect and leads to long workflow crossing services and rabbitmq, and a long workflow means it is difficult to code and more importantly difficult to test, for example, to approve a tree, we need to do the creation in `treetracker` db schema and emit an event to another microservice to inform it to do corresponding action (update some data in its domain/schema). In this proposal, I suggest direct access for some microservices to be able to finish the job in a single place, of course, with some rules to access.

Also, rabbitmq is a big burden for us to maintain and keep right, don't forget, rabbitmq stores data (events) in the system, it is a state of the system, so once we lost ( for example, the service gets evicted by k8s, or something wrong happened to the persistent storage of it), we are facing error of the whole system, not like microservices, they are stateless. I feel for now there isn't a member in the community could be able to really operate and maintain rabbitmq in a serious manner as a production service.


### Solution 

I suggest we make some changes to our architecture:

1. Moderately couple the microservices.

We should allow some microservices to have access to multiple database schemas for good enough reason, for example, the service to handle the tree and capture should be able to have access to raw capture schema to allow some necessary updates, just like the example that I gave above.

A bonus for this manner is that we can use database transactions here, to do a crossing schema update, it makes the system state more robust.

We are not saying we should allow any kind of access for services, the Machine Learning service should not have direct access to the core schema, and we still need a message queue to handle this, it means, we emit messages of like, new capture generation to queue, and the ML service listen to it and process the event.

To avoid unnecessary coupling, we need to define a clear relationship between the microservices, which service has access to which schemas, and use Terraform to strictly set the access in DB, by the role for every microservice. 

2. Use a simple queue to replace rabbitmq.

We can use our database Postgresql as a simple queue, there are already companies adopting this solution, and some libraries in different languages support this. Some resources here:

https://www.prequel.co/blog/sql-maxis-why-we-ditched-rabbitmq-and-replaced-it-with-a-postgres-queue

https://github.com/timgit/pg-boss/tree/master

In this way, we use a dedicated database schema to store the message, with simple API, microservice could send and subscribe message.

Also, we can put the queue operation into our database transaction, which means the whole process including queue emitting and database update will be atomic, which makes the process simple and safe.


#### The pros of this solution:

* It simplifies our microservices, easier to code and test, which means faster development, fewer bugs, robust system, the price for this is: higher coupling, lower throughout capacity, not suit a big and complicated business model, but I feel these problems are not the most priority for us by now.

* It is easier to maintain, we don't add any extra maintenance by doing this, the Postgresql is already there, and we removed rabbitmq.

* It is safer, all the states are in the database, as long as the database is safe and recoverable, we have all the states, even the whole k8s crashed.

* Being able to use transactions for a bunch of operations, is a big relief for developers from the burden of considering all kinds of exceptions and consequences by the possibility of unsafe operations.


#### The cons of this solution:

* The Postgresql message queue is not a full-featured message queue, for example, the pub/sub model could not be as powerful as rabbitmq.

* It is less decoupled

* It is less scalable, it can not support a use case that needs a huge message throughout capacity, and the queue is not a cluster, so we can not scale it easily. 

## Summary

By using a simple queue + moderately coupled microservices to streamline our platform, we can speed up our development on the new feature, and new domain model we planned, and easy to maintain ( without the burden of rabbitmq), and it's a safer solution on our data and application robustness.

The problem with this solution is the ability to scale up, but I think when we reach the ceiling of this solution, we are big enough to set up a big team, including cloud engineering to handle the platform, before that, this solution is a suitable choice for us. By the way, the rabbitmq is not really used currently, so it is easy for us to do the switch.



