# Scopes and Lifecycle

- [Overview](#overview)
  - [Bean Scopes](#bean-scopes)
  - [Additional Spring Bean Scopes](#additional-spring-bean-scopes)
  - [Bean Lifecycle](#bean-lifecycle)
- [Scopes and Lifecycle with XML Configuration](#scopes-and-lifecycle-with-xml-configuration)
  - [Scopes](#scopes)
  - [Lifecycle](#lifecycle)
- [Scopes and Lifecycle with Java Annotations](#scopes-and-lifecycle-with-java-annotations)
  - [Scopes](#scopes-1)
  - [Lifecycle](#lifecycle-1)

## Overview

### Bean Scopes

The **Bean Scope** refer to the lifecycle of a bean. It will determine how long the bean live, how many instance are created and how is the bean shared.

- The default scope is **Singleton**.
  - Spring Container creates only one instance of the bean, by default.
  - It's cached in memory.
  - All requests for the bean will return a SHARED reference to the SAME bean.
- Moreover, we can explicitly specify Bean Scope.

### Additional Spring Bean Scopes

```xml
<beans ...>
    <bean id="myCoach"
          class="com.luv2code.springdemo.TrackCoach"
          init-method="doMyStartupStuff"
          destroy-method="doMyCleanupStuff">
          ...
    </bean>
</beans>
```

We can explicitly specify Bean Scope in the XML config file. Let's see the Spring Bean Scopes:

|      Scope       | Description                                                          |
| :--------------: | :------------------------------------------------------------------- |
|   `singleton`    | Create a single shared instance of the bean. (DEFAULT)               |
|   `prototype`    | Create a new bean instance for each container request.               |
|    `request`     | Scoped to an HTTP web request. Only used for web applications.       |
|    `session`     | Scoped to an HTTP web session. Only used for web applications.       |
| `global-session` | Scoped to a global HTTP web session. Only used for web applications. |

In contrast to the other scopes, Spring does not manage the complete lifecycle of `prototype` beans. As it to say, Spring does not call the destroy method for the `prototype` scoped beans.

### Bean Lifecycle

```xml
<beans ...>
    <bean id="myCoach"
          class="com.luv2code.springdemo.TrackCoach"
          scope="singleton">
          ...
    </bean>
</beans>
```

When the Spring Container starts, there are a couple of things that happens:

<div align="center">
  <img src="https://i.imgur.com/4IsgFq2.png" alt="Bean Lifecycle">
</div>

Notice that we can add custom Bean Lifecycle Methods/Hooks during **bean initialization** and **bean destruction**.

- Calling custom business logic method
- Setting up handles to resource (db, sockets, files, etc)

We can use annotations `@PostConstruct` and `@PreDestroy` for adding those methods.

<br/>
<div align="right">
  <b><a href="#scopes-and-lifecycle">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Scopes and Lifecycle with XML Configuration

### Scopes

1. Create Configuration File

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
                               http://www.springframework.org/schema/beans/spring-beans.xsd
                               http://www.springframework.org/schema/context
                               http://www.springframework.org/schema/context/spring-context.xsd">

        <!-- define the dependency-->
        <bean id="myFortuneService" class="com.luv2code.springdemo.HappyFortuneService"></bean>

        <!-- define your beans here -->
        <bean id="myCoach" class="com.luv2code.springdemo.TrackCoach" scope="prototype">
            <!-- set up constructor injection -->
            <constructor-arg ref="myFortuneService" />
        </bean>
    </beans>
    ```

2. Check the Instance

    ```java
    package com.luv2code.springdemo;

    import org.springframework.context.support.ClassPathXmlApplicationContext;

    public class BeanScopeDemoApp {
        public static void main(String[] args) {
            // load the spring configuration file
            ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beanScope-applicationContext.xml");

            // retrieve bean from spring container
            Coach theCoach = context.getBean("myCoach", Coach.class);
            Coach alphaCoach = context.getBean("myCoach", Coach.class);

            // check if they are the same
            boolean result = (theCoach == alphaCoach);
            System.out.println("\nPointing to theCoach same object: " + result);
            System.out.println("\nMemory location for theCoach: " + theCoach);
            System.out.println("\nMemory location for alphaCoach: " + alphaCoach);

            // close the context
            context.close();
        }
    }
    ```

### Lifecycle

1. Define the Methods for Init and Detroy.

    ```java
    package com.luv2code.springdemo;

    public class TrackCoach implements Coach {
        // define a private field for the dependency
        private FortuneService fortuneService;

        // define a constructor for dependency injection
        public TrackCoach(FortuneService theFortuneService) {
            fortuneService = theFortuneService;
        }

        @Override
        public String getDailyWorkout() {
            return "Run a hard 5k";
        }

        @Override
        public String getDailyFortune() {
            return "Just do it: " + fortuneService.getFortune();
        }

        // add an init method
        public void doMyStartupStuff() {
            System.out.println("TrackCoach: inside method doMyStartupStuff");
        }

        // add a destroy method
        public void doMyCleanupStuffYoYo() {
            System.out.println("TrackCoach: inside method doMyCleanupStuffYoYo");
        }
    }
    ```

2. Configure the Method Names in Spring Config File

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
                              http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/context
                              http://www.springframework.org/schema/context/spring-context.xsd">

        <!-- define the dependency-->
        <bean id="myFortuneService" class="com.luv2code.springdemo.HappyFortuneService"></bean>

        <!-- define your beans here -->
        <bean id="myCoach" class="com.luv2code.springdemo.TrackCoach"
              init-method="doMyStartupStuff"
              destroy-method="doMyCleanupStuffYoYo">
            <!-- set up constructor injection -->
            <constructor-arg ref="myFortuneService" />
        </bean>
    </beans>
    ```

<br/>
<div align="right">
  <b><a href="#scopes-and-lifecycle">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Scopes and Lifecycle with Java Annotations

### Scopes

```java
package com.luv2code.springdemo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component
@Scope("prototype")
public class TennisCoach implements Coach {
    @Autowired
    @Qualifier("randomFortuneService")
    private FortuneService fortuneService;

    public TennisCoach() {
        System.out.println(">> TennisCoach: inside default constructor");
    }

    @Override
    public String getDailyWorkout() {
        return "Practice your backhand volley";
    }

    @Override
    public String getDailyFortune() {
        return fortuneService.getFortune();
    }
}
```

### Lifecycle

```java
package com.luv2code.springdemo;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Component;

@Component
public class TennisCoach implements Coach {
    @Autowired
    @Qualifier("randomFortuneService")
    private FortuneService fortuneService;

    // define a default constructor
    public TennisCoach() {
        System.out.println(">> TennisCoach: inside default constructor");
    }

    // define my init method
    @PostConstruct
    public void doMyStartupStuff() {
        System.out.println(">> TennisCoach: inside of doMyStartupStuff()");
    }

    // define my destroy method
    @PreDestroy
    public void doMyCleanupStuff() {
        System.out.println(">> TennisCoach: inside of doMyCleanupStuff()");
    }

    @Override
    public String getDailyWorkout() {
        return "Practice your backhand volley";
    }

    @Override
    public String getDailyFortune() {
        return fortuneService.getFortune();
    }
}
```

<br/>
<div align="right">
  <b><a href="#scopes-and-lifecycle">[ ↥ Back To Top ]</a></b>
</div>
<br/>