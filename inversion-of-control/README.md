# Inversion of Control (IoC)

- [Overview](#overview)
  - [Inversion of Control](#inversion-of-control)
  - [Class and Interface](#class-and-interface)
  - [Spring Bean](#spring-bean)
- [Inversion of Control in Spring](#inversion-of-control-in-spring)
  - [Primary Functions of Sprint Container](#primary-functions-of-sprint-container)
  - [Ways of Configuring the Spring Container](#ways-of-configuring-the-spring-container)
  - [Spring IoC Step by Step (XML File)](#spring-ioc-step-by-step-xml-file)
  - [Spring IoC Step by Step (Java Annotations)](#spring-ioc-step-by-step-java-annotations)
  - [Spring IoC Step by Step (Java Configuration Class)](#spring-ioc-step-by-step-java-configuration-class)
- [Inversion of Control with XML Configuration](#inversion-of-control-with-xml-configuration)
  - [Configure the Spring Beans](#configure-the-spring-beans)
  - [Create Spring Container and Retrieve Bean from Spring Container](#create-spring-container-and-retrieve-bean-from-spring-container)
- [Inversion of Control with Java Annotations](#inversion-of-control-with-java-annotations)
  - [Enable Component Scanning in Spring Config File](#enable-component-scanning-in-spring-config-file)
  - [Add the `@Component` Annotation to the Java Classes](#add-the-component-annotation-to-the-java-classes)
  - [Retrieve Bean from Spring Container](#retrieve-bean-from-spring-container)

## Overview

### Inversion of Control

**Inversion of Control (IoC)** is the approach of outsourcing the construction and management of objects. And the outsourceing will be handled by an object factory.

### Class and Interface

```java
// [FILE] MyApp.java
package com.luv2code.springdemo;

public class MyApp {
    public static void main(String[] args) {
        // create the object
        Coach theCoach = new TrackCoach();

        // use the object
        System.out.println(theCoach.getDailyWorkout());
  }
}
```

```java
// [File] Coach.java
package com.luv2code.springdemo;

public interface Coach {
    public String getDailyWorkout();
}
```

```java
// [File] BaseballCoach.java
package com.luv2code.springdemo;

public class BaseballCoach implements Coach {
    @Override
    public String getDailyWorkout() {
        return "Spend 30 minutes on batting practice";
    }
}
```

```java
// [File] TrackCoach.java
package com.luv2code.springdemo;

public class TrackCoach implements Coach {
    @Override
    public String getDailyWorkout() {
        return "Run a hard 5k";
    }
}
```

### Spring Bean

Let's see the blurb from the [Spring Reference Manual](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction):

> In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and managed by a Spring IoC container. Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.

- Whenever we see **Spring Bean**, just think it as Java Objects.
- When Java objects are created by the Spring Container, then Spring refers to them as **Spring Beans**.
- **Spring Beans** are created from normal Java classes just like Java objects.

<br/>
<div align="right">
  <b><a href="#inversion-of-control-ioc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Inversion of Control in Spring

### Primary Functions of Sprint Container

The primary functions of Spring Container are:

- Create and manage objects (Inversion of Control, IoC)
- Inject object's dependencies (Dependency Injection, DI)

### Ways of Configuring the Spring Container

There're three ways of configuring the Spring Container:

1. XML Configuration file (legacy, but most legacy applications still use this)
2. Java Annotations (modern)
    - The special lables/markers added to Java classes.
    - Provide meta-data about the class.
    - Processed at compile time or run-time for special processing.
    - Spring will scan the Java classes for special annotations and register the beans in the Spring Container automatically.
3. Java Source Code (modern)

### Spring IoC Step by Step (XML File)

The development process of Spring IoC can be summaried as steps below:

1. Configure the Spring Beans.
2. Create a Spring Container.
    - In the Spring world, a Spring Container is generically known as `ApplicationContext`.
    - e.g. `ClassPathXmlApplicationContext`, `AnnotationConfigApplicationContext`, `GenericWebApplicationContext`, others...
3. Retrieve Beans from Spring Container.

### Spring IoC Step by Step (Java Annotations)

The development process of Spring IoC can be summaried as steps below:

1. Enable Component Scanning in Spring Config File.
2. Add the `@Component` Annotation to the Java Classes.
3. Retrieve Beans from Spring Container.

### Spring IoC Step by Step (Java Configuration Class)

The development process of Spring IoC can be summaried as steps below:

1. Create Java Class and Annotate as `@Configuration`.
2. Add Component Scanning Support with `@ComponentScan` (optional).
3. Read Spring Java Configuration Class.
4. Retrieve Bean from Spring Container.

<br/>
<div align="right">
  <b><a href="#inversion-of-control-ioc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Inversion of Control with XML Configuration

### Configure the Spring Beans

First, add the `applicationContext.xml` to the `src` folder:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- Define your beans here -->
    <bean id="myCoach"
          class="com.luv2code.springdemo.TrackCoach">
    </bean>
</beans>
```

- The `id` is like an alias.
- The `class` should be the fully qualified class name of implementation class.

### Create Spring Container and Retrieve Bean from Spring Container

Now, load the spring configuration file and retrieve bean from the spring container:

```java
package com.luv2code.springdemo;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class HelloSpringApp {
    public static void main(String[] args) {
        // load the spring configuration file
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        // retrieve bean from spring container
        Coach theCoach = context.getBean("myCoach", Coach.class);

        // call methods on the bean
        System.out.println(theCoach.getDailyWorkout());

        // close the context
        context.close();
    }
}
```

- The first parameter in `context.getBean()` should be the bean id.
- The second parameter in `context.getBean()` should be the interface.
- When we pass the interface to the method, Spring will cast the object for us behind the scenes.

<br/>
<div align="right">
  <b><a href="#inversion-of-control-ioc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Inversion of Control with Java Annotations

### Enable Component Scanning in Spring Config File

Add `applicationContext.xml` file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- add entry to enable component scanning -->
    <context:component-scan base-package="com.luv2code.springdemo" />
</beans>
```

- The `base-package` should be the name of the package.

### Add the `@Component` Annotation to the Java Classes

Create a package named `com.luv2code.springdemo` and interface `Coach.java`:

```java
package com.luv2code.springdemo;

public interface Coach {
    public String getDailyWorkout();
}
```

Then create the class `TennisCoach.java` implementing interface `Coach` and add the `@Component` annotation:

```java
package com.luv2code.springdemo;

import org.springframework.stereotype.Component;

@Component("thatSillyCoach")
public class TennisCoach implements Coach {
    @Override
    public String getDailyWorkout() {
        return "Practice your backhand volley";
    }
}
```

- The `"thatSillyCoach"` would be the bean id.
- If we don't give the bean id, the default bean id would be the class name but making first letter lower-case. Special case when BOTH the first and second characters of the class name are upper case, then the name is NOT converted.
  - e.g. The default bean id of class `TennisCoach` would be `tennisCoach`.
  - e.g. The default bean id of class `RESTFortuneService` would be `RESTFortuneService`.

### Retrieve Bean from Spring Container

```java
package com.luv2code.springdemo;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class AnnotationDemoApp {
    public static void main(String[] args) {
        // read spring config file
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        // get the bean from spring container
        Coach theCoach = context.getBean("thatSillyCoach", Coach.class);

        // call a method on the bean
        System.out.println(theCoach.getDailyWorkout());

        // close the context
        context.close();
    }
}
```

<br/>
<div align="right">
  <b><a href="#inversion-of-control-ioc">[ ↥ Back To Top ]</a></b>
</div>
<br/>