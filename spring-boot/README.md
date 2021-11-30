# Spring Boot

- [Spring Boot](#spring-boot)
  - [Overview](#overview)
    - [Spring Boot](#spring-boot-1)
    - [Spring Initializr](#spring-initializr)
    - [Spring Boot Project Structure](#spring-boot-project-structure)
    - [Spring Boot Starts](#spring-boot-starts)
  - [Running Spring Boot Apps from Command Line](#running-spring-boot-apps-from-command-line)
  - [Application Properties](#application-properties)
  - [Spring Data JPA](#spring-data-jpa)
    - [Overview](#overview-1)
  - [Spring Data REST](#spring-data-rest)
  - [Thymeleaf](#thymeleaf)
    - [Overview](#overview-2)
    - [Add Thymeleaf to Maven `pom.xml` File](#add-thymeleaf-to-maven-pomxml-file)
    - [Develop Spring MVC Controller](#develop-spring-mvc-controller)
    - [The Thymeleaf Template](#the-thymeleaf-template)

## Overview

### Spring Boot

The **Spring Boot** is a solution to make it easier to get started with Spring development:

- Minimize the amount of manual configuration for us by performing auto-configuration based on props files and JAR classpath.
- Help to resolve dependecy conflicts (even Maven or Gradle).
- Provide an embedded HTTP server (Tomcat, Jetty, Undertow, ...) and we don't need to install a server separately.

### Spring Initializr

The [Spring Initializr](https://start.spring.io) is the website provided by Spring Boot project, it help developers quickly create a starter Spring project.

We can setup the project information and download the `*.zip` file. The file can be imported with any IDE or editor as we like. There are something to be awared when importing with Eclipse:

- Add `<maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>` in the `pom.xml` and update project with Maven.
- Choose [Run As] > [Java Application] (not "Run on Server") to run the application.

### Spring Boot Project Structure

### Spring Boot Starts

<br/>
<div align="right">
  <b><a href="#spring-boot">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Running Spring Boot Apps from Command Line

There are two options for running the application with command line:

1. Use Java
2. Use Spring Boot Maven Plugin
    - No need to have Maven installed or present on the path.
    - If correct version of Maven is Not found on the computer, it will automatically download Maven.
    - If we already have Maven installed previously, we can ignore or delete the `mvnw` files.

```bash
# [Option 1] Java
$ java -jar <NAME_OF_JAR>.jar

# [Option 2] Spring Boot Maven Plugin
$ mvnw clean compile test
$ mvnw package
$ mvnw spring-boot:run
```

<br/>
<div align="right">
  <b><a href="#spring-boot">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Application Properties

By default, Spring Boot reads information from a standard properties file loacted at `./src/main/resources/application.properties`. We can define ANY custom properties in the file and then access these properties using `@Value` annotations in application.

1. Define Custom Application Properties

    ```
    # Define Custom Properties
    coach.name=Mickey Mouse
    team.name=The Mouse Club
    ```

2. Inject Properties into Spring Boot App

```java
@RestController
public class FunRestController {
    // inject properties for: coach.name and team.name
    @Value("${coach.name}")
    private String coachName;

    @Value("${team.name}")
    private String teamName;
    ...
}
```

<br/>
<div align="right">
  <b><a href="#spring-boot">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Spring Data JPA

### Overview

**Spring Data JPA** create a DAO (Data Access Object) and just plug in our entity type and primary key. Then the Spring will give us a CRUD implementation for minimizing boiler-plate DAO code.

- Spring Data JPA provides the interface `JpaRepository`.
- It exposes `findAll()`, `findById()`, `save()`, `deleteById()` and others.


<br/>
<div align="right">
  <b><a href="#spring-boot">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Spring Data REST

<br/>
<div align="right">
  <b><a href="#spring-boot">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Thymeleaf

### Overview

**Thymeleaf** is a Java templating engine commonly used to generate the HTML views for web applications. Moreover, it's a general purpose templating engine that can also used outside of web applications.

- Can be an HTML page with some Thymeleaf expressions.
- Include dynamic content from Thymeleaf expressions.
- Also can be used as Email Templates, CSV Template and PDF Template.

### Add Thymeleaf to Maven `pom.xml` File

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

Spring will auto-configure to use Thymeleaf since we have Thymeleaf dependency in Maven POM.

### Develop Spring MVC Controller

```java
@Controller
public class DemoController {
    @GetMapping("/")
    public String sayHello(Model theModel) {
        theModel.addAttribute("theData", new java.util.Date());
        return "helloworld";
    }
}
```

- In Spring, the Thymeleaf template files go in `./src/main/resources/templates`.
- For web applications, Thymeleaf templates have a `*.html` extension. e.g. `helloworld.html`.

### The Thymeleaf Template

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
  <head> ... </head>
  <body>
    <p th:text="'Time on the server is ' + ${theDate}" />
  </body>
</html>
```

<br/>
<div align="right">
  <b><a href="#spring-boot">[ ↥ Back To Top ]</a></b>
</div>
<br/>
