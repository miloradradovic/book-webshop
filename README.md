Introduction
============

Book webshop is a Spring Boot application with microservice architecture. It consists of 5 Spring Boot apps which act as separate microservices. They will be explained in the following sections.
Application supports display of a book catalog, as well as creating and monitoring orders. 

Application general info
========================

As mentioned, there are 5 microservices that are used to build the application: book-webshop-eureka, book-webshop-api-gateway, book-webshop-auth, book-webshop-catalog and book-webshop-order. MySQL was used for a database(the idea was to create schema per service). All the business logic services are registered to spring cloud eureka server and use open feign client to communicate with each other when needed.

[book-webshop-eureka](https://github.com/miloradradovic/book-webshop-eureka)
=====================
