Introduction
============

Book webshop is a Spring Boot application with microservice architecture. It consists of 5 Spring Boot apps which act as separate microservices and 1 Angular app which acts as a client. They will be explained in the following sections.
Application supports display of a book catalog, placing and monitoring orders, as well as basic admin crud functionalities. 
Deployed using [Kubernetes](https://kubernetes.io/).

Architecture and basic info
=================
As mentioned, there are 5 microservices that are used to build the application: book-webshop-eureka, book-webshop-api-gateway, book-webshop-auth, book-webshop-catalog and book-webshop-order, as well as one Angular application which will be used as a client - book-webshop-client. MySQL was used for a database (schema-per-service pattern). Notably, there is a separate git repo for kubernetes scripts. All the business logic services are registered to Spring Cloud Eureka server and use [Spring Cloud OpenFeign](https://spring.io/projects/spring-cloud-openfeign) client to communicate with each other when needed. [Spring Security](https://spring.io/projects/spring-security) with [JWT](https://jwt.io/) was used to secure api endpoints.
Every one of the mentioned applications is dockerized and deployed on local kubernetes cluster.

[book-webshop-eureka](https://github.com/miloradradovic/book-webshop-eureka)
=====================
[Spring Cloud Netflix Eureka](https://spring.io/projects/spring-cloud-netflix) server is an application which holds information about other applications registered to it (in this context - eureka clients). Book-webshop-eureka has that purpose in this book webshop architecture. It registers other microservices so every one of them can interact with each other without knowing the basic information like port or ip address. For example, [here](https://github.com/miloradradovic/book-webshop-order/blob/main/src/main/java/com/example/orderservice/feign/BookFeign.java) - order service doesn't know about catalog-server's port, but it still can interact with it (in this case, through feign client). Docker image can be found [here](https://hub.docker.com/r/comeee98/book-webshop-eureka).

[book-webshop-api-gateway](https://github.com/miloradradovic/book-webshop-api-gateway)
==========================
This application uses [Spring Cloud Gateway](https://spring.io/projects/spring-cloud-gateway) as its base. The purpose of a gateway is to act as a firewall between client and server. In other words, client doesn't need to know the port of a service that needs to be used. Instead - it will just hit the gateway, which will redirect the request to the right service. Docker image can be found [here](https://hub.docker.com/r/comeee98/book-webshop-api-gateway).

[book-webshop-auth](https://github.com/miloradradovic/book-webshop-auth)
===================
Auth server is a spring boot application used for authentication and authorization, as well as user management. It holds information about User entities in its database schema. It supports the functionalities of signing in, registering, jwt token management and crud operations for user entity. Docker image can be found [here](https://hub.docker.com/r/comeee98/book-webshop-auth).

[book-webshop-catalog](https://github.com/miloradradovic/book-webshop-catalog)
======================
Catalog server is a simple spring boot application used for managing webshop's catalog of books. Books with their respective writers are stored in its database schema. Currently it has the functionalities of fetching the whole catalog, some apis that will be used by other microservices (like updating book's in stock value after order is placed), as well as basic crud operations for entities. Docker image can be found [here](https://hub.docker.com/r/comeee98/book-webshop-catalog).

[book-webshop-order](https://github.com/miloradradovic/book-webshop-order)
====================
Order server acts as an order management application. It holds the order information in its database schema. Currently it has functionalities of placing orders, managing their status, basic crud operations as well as order status updating simulation. Docker image can be found [here](https://hub.docker.com/r/comeee98/book-webshop-order).

[book-webshop-client](https://github.com/miloradradovic/book-webshop-client)
=====================
Book-webshop-client is an Angular application which acts as a client. It consists of 3 feature modules:
  - Shared - has declarations of components which will be used throughout the application. For example: table component for visualizing data.
  - Pages - has declarations of components that represent pages and dialogs which will interact with the end user.
  - Material - has declarations of [Angular Material](https://material.angular.io/) components.
Apart from the feature modules, application also has [guards](https://angular.io/api/router/CanActivate), [services](https://angular.io/guide/architecture-services), model (classes that represent the model on the server side), [interceptor](https://angular.io/api/common/http/HttpInterceptor) and routing files. Docker image can be found [here](https://hub.docker.com/r/comeee98/book-webshop-client).

[book-webshop-k8s](https://github.com/miloradradovic/book-webshop-k8s)
==================
As mentioned beforehand, there is a separate git repo for kubernetes deployment scripts. This repo consists of 2 directories: deployments and utils. Utils scripts should be deployed first since they are mainly config scripts (like configmaps, mysql persistence volume and secrets), while deployments scripts are all scripts for deploying each microservice.

[photo-upload](https://github.com/miloradradovic/photo-upload)
==============
Every book entity has its photo that will be displayed on the client side. Uploading and retrieving photos for books was accomplished by implementing a separate photo-upload Spring Boot service. It receives multipart data from client and stores photos in resources directory of the application. It also provides the functionality of getting photo by name, as well as deleting photo by name. Docker image can be found [here](https://hub.docker.com/r/comeee98/photo-upload).
