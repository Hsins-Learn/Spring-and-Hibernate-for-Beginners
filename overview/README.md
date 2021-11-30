# Overview

- [Why Spring?](#why-spring)
  - [Spring in a Nutshell](#spring-in-a-nutshell)
  - [Java Platform Enterprise Edition (J2EE/Java EE)](#java-platform-enterprise-edition-j2eejava-ee)
- [Spring 5 Update](#spring-5-update)
- [Spring Core Framework](#spring-core-framework)
  - [Goals of Spring](#goals-of-spring)
  - [Framework Overview](#framework-overview)
- [Spring Platform](#spring-platform)

## Why Spring?

### Spring in a Nutshell

- Very popular framework for building Java applications.
- Initially a simpler and lightweight alternative to J2EE (Java EE).
- Provides a large number of helper classes to make things easier.
- We should learn both Java EE and Spring now, and lots of skills are interchangeable.

### Java Platform Enterprise Edition (J2EE/Java EE)

- Contains (1) Client-Side Presentation, (2) Server-Side Presentation, (3) Server-Side Business Logic and (4) Database.
- History of Java EE
  - (1999) J2EE 1.2: Servlets, JSP, EJB, JMS, RMI
  - (2001) J2EE 1.3: EJB, CMP, JCA
  - (2003) J2EE 1.4: Web Services, Management, Deployment
  - (2006) Java EE 5: EJB 3, JPA, JSF, JAXB, JAX-WS
  - (2009) Java EE 6: JAX-RS, CDI, Bean-Validation
  - (2013) Java EE 7: JMS 2, Batch, TX, Concurrency, Web Sockets
  - (2017) Java EE 8
  - (2018) Jakarta EE

<br/>
<div align="right">
  <b><a href="#overview">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Spring 5 Update

- Updated minimum requirements for Java 8 or higher.
- Deprecated legacy integration for: Tiles, Velocity, Portlet, Guava etc.
- Upgraded Spring MVC to use new versions of Servlet API 4.0.
- Added new reactive programming framework: Spring WebFlux.

<br/>
<div align="right">
  <b><a href="#overview">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Spring Core Framework

### Goals of Spring

- Lightweight development with Java POJOs (Plain-Old-Java-Objects)
- Dependency injection to promote loose coupling.
- Declarative programming with Aspect-Oriented-Programming (AOP).
- Minimize boilerplate Java code.

### Framework Overview

- **Core Container**
  - Contains Beans, Core, SpEL and Context.
  - Factory for creating beans.
  - Manage bean dependencies.
- **AOP Infrastructure**
  - AOP means Aspect Oriented Programming.
  - To add functionality to objects declaratively like loggings, security, transactions, etc...
- **Data Access Layer (Intergration)**
  - The layer is for communicating with the database, either a relational database or a NoSQL database.
  - Provides helper classes to make it much easier to access a database using JDBC.
  - Use ORM (Object to Relational Mapping) integration with Hibernate and JPA.
  - Provides helper classes for sending async messages to a Message Broker with JMS (Java Message Service).
- **Web Layer**
  - Contains all web related classes, it's the home of the Spring MVC framework.
- **Test Layer**
  - Supports Test-Driven-Development (TDD).
  - Mock objects and out-of-container testing.

<br/>
<div align="right">
  <b><a href="#overview">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Spring Platform

Spring Prjects are additional Spring modules built-on top of the core Spring Framekwork. We can use what we need:

- Spring Cloud, Spring Data
- Spring Batch, Spring Security
- Spring for Android, Spring Web Flow
- Spring Web Services, Spring LDAP

<br/>
<div align="right">
  <b><a href="#overview">[ ↥ Back To Top ]</a></b>
</div>
<br/>