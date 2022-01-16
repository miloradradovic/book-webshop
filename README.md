Introduction
============

Book webshop is a Spring Boot application with microservice architecture. It consists of 5 Spring Boot apps which act as separate microservices. They will be explained in the following sections.
Application supports display of a book catalog, as well as creating and monitoring orders. 
Will be deployed using [Kubernetes](https://kubernetes.io/).

Architecture and basic info
=================

As mentioned, there are 5 microservices that are used to build the application: book-webshop-eureka, book-webshop-api-gateway, book-webshop-auth, book-webshop-catalog and book-webshop-order. MySQL was used for a database (the idea was to create schema per service). Notably, there is a separate git repo for kubernetes scripts. All the business logic services are registered to Spring Cloud Eureka server and use [Spring Cloud OpenFeign](https://spring.io/projects/spring-cloud-openfeign) client to communicate with each other when needed. [Spring Security](https://spring.io/projects/spring-security) with [JWT](https://jwt.io/) were used to secure api endpoints.
Every microservice git repo has 3 branches - main, develop and test. Test, as the name suggests, is where unit tests are being written. Develop branch has all the code which is to be deployed. Main branch will have the final dockerized and deployed code.

[book-webshop-eureka](https://github.com/miloradradovic/book-webshop-eureka)
=====================
[Spring Cloud Netflix Eureka](https://spring.io/projects/spring-cloud-netflix) server is an application which holds information about other applications registered to it (in this context - eureka clients). Book-webshop-eureka has that purpose in this book webshop architecture. It registers other microservices so every one of them can interact with each other without knowing the basic information like port or ip address. For example, [here](https://github.com/miloradradovic/book-webshop-order/blob/develop/src/main/java/com/example/orderservice/feign/BookFeign.java) - order service doesn't know about catalog-server's port, but it still can interact with it (in this case, through feign client).

[book-webshop-api-gateway](https://github.com/miloradradovic/book-webshop-api-gateway)
==========================
This application uses [Spring Cloud Gateway](https://spring.io/projects/spring-cloud-gateway) as its base. The purpose of a gateway is to act as a firewall between client and server. In other words, client doesn't have to know the port of a service that needs to be used. Instead - it will just hit the gateway, which will redirect the request to the right service.

[book-webshop-auth](https://github.com/miloradradovic/book-webshop-auth)
===================
Auth server is a spring boot application used for authentication and authorization, as well as user management. It holds information about User entities in its database schema. Currently it has the functionalities of signing in, registering and crud operations for user entity.

[book-webshop-catalog](https://github.com/miloradradovic/book-webshop-catalog)
======================
Catalog server is a simple spring boot application used for managing webshop's catalog of books. Books with their respective writers are stored in its database schema. Currently it has the functionalities of fetching the whole catalog, some apis that will be used by other microservices (like updating book's in stock value after order is placed), as well as basic crud operations for entities.

[book-webshop-order](https://github.com/miloradradovic/book-webshop-order)
====================
Order server will act as an order management application. It holds the order information in its database schema. Currently it has functionalities of placing orders, managing their status, basic crud operations as well as order status updating simulation.

[book-webshop-k8s](https://github.com/miloradradovic/book-webshop-k8s)
==================
As mentioned beforehand, there is a separate git repo for kubernetes deployment scripts. This repo consists of 2 directories: deployments and utils. Utils scripts should be deployed first since they are mainly config scripts (like configmaps, mysql persistence volume and secrets), while deployments scripts are all scripts for deploying each microservice.
