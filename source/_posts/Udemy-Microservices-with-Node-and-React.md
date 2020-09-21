---
title: Udemy - Microservices with Node and React
date: 2020-09-18 14:09:09
tags:
- Microservice
- Node
- React
categories:
- Full Stack
---


# Microservice 

This course introduces the concept of microservices and the application using Node.js and React.

This course can be found on Udemy, click the link here.

## What is Microservice 

From what we know about traditional structure, we have the monolithic server structure, as illustrated below:

![Monolithic Server](monolithic_server.png)

In a monolithic server, we have our requests go through each the auth middleware, router, feature and then to the data. The problem is that if one feature is down, or something wrong with the database, 

and this is called the microservices

![Microservices](microservice_def.png)

A single microservices contains 4 components:

* Routing 
* Middlewares
* Business Logic 
* Database Access 

All to, compare to the monolithic structure, implement ***one feature*** of our app. 

The following illustration can give a better understanding of this difference: 

![Monolithic Server](microservice_illustration.png)

Here the request will traverse each service, and each service is independent to each other to make sure, if others fail, it will keep working.

The reason to use microservices is simple, we tend to breakdown

![Monolithic Server](monolithic_server.png)

The biggest problem in the microservices is the data management


There are 2 types of communications in microservice architecture:

* Synchronous 
* Asynchronous 
  
![sync and async](15.png)

details can be found [here](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/architect-microservice-container-applications/communication-in-microservice-architecture)



First, synchronous communication has the following pros and cons:


![sync pros and cons](13.png)

There are 2 types of methods in asynchronous communication. The following is the 1st one.

![async 1st way](2.png)

passing events to event bus and then delivered to other 
![async 1st way overall](5.png)




The second method works like the database-per-service pattern, we create a database for service D and store information in it.


![async 2nd way](4.png)


![async 2nd way overall](7.png)






![async pros and cons](10.png)




It is actually not hard to unders

![extra costs](6.png)



## Example App Using the Microservice Architecture 

![example app setup](12.png)

![example app components](11.png)


![example app components functions](8.png)


