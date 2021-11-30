# Dependency Injection (DI)

- [Overview](#overview)
  - [Dependency Injection](#dependency-injection)
  - [Spring AutoWiring: `@Autowired` and `@Qualifier`](#spring-autowiring-autowired-and-qualifier)
- [Dependency Injection in Spring](#dependency-injection-in-spring)
  - [Injection Types](#injection-types)
  - [Spring DI Step by Step (XML File)](#spring-di-step-by-step-xml-file)
  - [Spring DI Step by Step (Java Annotation)](#spring-di-step-by-step-java-annotation)
- [Dependency Injection with XML Configuration (Constructor Injection)](#dependency-injection-with-xml-configuration-constructor-injection)
  - [Define the Interface and Class](#define-the-interface-and-class)
  - [Create Constructor in the Class for Injection](#create-constructor-in-the-class-for-injection)
  - [Configure the Dependency Injection in Spring Config File](#configure-the-dependency-injection-in-spring-config-file)
- [Dependency Injection with XML Configuration (Setter Injection)](#dependency-injection-with-xml-configuration-setter-injection)
  - [Create Setter Methods in the Class for Injections](#create-setter-methods-in-the-class-for-injections)
  - [Configure the Dependency Injection in Spring Config File](#configure-the-dependency-injection-in-spring-config-file-1)
  - [Demo with Setter Injections](#demo-with-setter-injections)
  - [Inject Literal Values](#inject-literal-values)
  - [Inject Values from a Properties File](#inject-values-from-a-properties-file)
- [Dependency Injection with Java Annotations (Constructor Injection)](#dependency-injection-with-java-annotations-constructor-injection)
  - [Define the Dependency Interface and Class](#define-the-dependency-interface-and-class)
  - [Create Constructor in the Class for Injections and Add `@Autowired` Annotation](#create-constructor-in-the-class-for-injections-and-add-autowired-annotation)
- [Dependency Injection with Java Annotations (Setter Injection)](#dependency-injection-with-java-annotations-setter-injection)
- [Dependency Injection with Java Annotations (Field Injection)](#dependency-injection-with-java-annotations-field-injection)
- [Dependency Injection with Java Configuration Class](#dependency-injection-with-java-configuration-class)
  - [Define Method on Expose Bean and Inject Bean Depencies](#define-method-on-expose-bean-and-inject-bean-depencies)
  - [Modify the Config File](#modify-the-config-file)
  - [Behind the Scenes](#behind-the-scenes)
  - [Inject Values from a Properties File](#inject-values-from-a-properties-file-1)

## Overview

### Dependency Injection

**Dependency Injection (DI)** allows a program design to follow the dependency inversion principle. The client will delegate to call to another object the responsibility of providing its dependencies.

### Spring AutoWiring: `@Autowired` and `@Qualifier`

Spring can use **Auto Wiring** for dependency injection.

- Spring will look for a class that matches the property. (matches by type: class or interface)
- Spring will inject it automatically hence it is autowired.

> **Example**: Injecting `FortuneService` into a `Coach` implementation
> 1. Spring will scan `@Components`
> 2. Any one implemenets `FortuneService` interface?
> 3. If so, let's inject them. e.g. `HappyFortuneService`.

If there are multiple implementations, we need to tell Spring which bean to use by `@Qualifier` annotations.

<br/>
<div align="right">
  <b><a href="#dependency-injection-di">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Dependency Injection in Spring

### Injection Types

There're many types of injection with Spring:

- **Constructor Injection**: inject dependencies by calling constructor of the class.
- **Setter Injection**: inject dependencies by calling setter methods on the class.
- **Field Injections**: inject dependencies by setting the field values on the class. (even private fields)

### Spring DI Step by Step (XML File)

The development process of Spring DI can be summaried as steps below:

1. Define the dependency interface and class.
2. Create a constructor or setter methods in our class for injections.
3. Configure the dependency injection in Spring config file.

### Spring DI Step by Step (Java Annotation)

The development process of Spring DI can be summaried as steps below:

1. Define the dependency interface and class.
2. Create a constructor in the class for injections.
3. Configure the dependency injection with `@Autowired` Annotation.

<br/>
<div align="right">
  <b><a href="#dependency-injection-di">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Dependency Injection with XML Configuration (Constructor Injection)

### Define the Interface and Class

First create the `FortuneService` interface:

```java
// [FILE] FortuneService.java
package com.luv2code.springdemo;

public interface FortuneService {
    public String getFortune();
}
```

Then create `HappyFortuneService` class which implement the `FortuneService` interface:

```java
// [FILE] HappyFortuneService.java
package com.luv2code.springdemo;

public class HappyFortuneService implements FortuneService {
    @Override
    public String getFortune() {
        return "Today is your lucky day!";
    }
}
```

Add the `getFortune()` method to `Coach` interface:

```java
package com.luv2code.springdemo;

public interface Coach {
    public String getDailyWorkout();
    public String getDailyFortune();
}
```

### Create Constructor in the Class for Injection

```java
// [FILE] BaseballCoach
package com.luv2code.springdemo;

public class BaseballCoach implements Coach {
    // define a private field for the dependency
    private FortuneService fortuneService;

    // define a constructor for dependency injection
    public BaseballCoach(FortuneService theFortuneService) {
        fortuneService = theFortuneService;
    }

    @Override
    public String getDailyWorkout() {
        return "Spend 30 minutes on batting practice";
    }

    @Override
    public String getDailyFortune() {
        // use my fortuneService to get a fortune
        return fortuneService.getFortune();
    }
}
```

### Configure the Dependency Injection in Spring Config File

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- Define the dependency-->
    <bean id="myFortuneService" class="com.luv2code.springdemo.HappyFortuneService">
    </bean>

    <!-- Define your beans here -->
    <bean id="myCoach" class="com.luv2code.springdemo.TrackCoach">
        <!-- Set up constructor injection -->
        <constructor-arg ref="myFortuneService" />
    </bean>
</beans>
```

- The `id` is like an alias.
- The `class` should be the fully qualified class name of implementation class.
- The no-arg constructor
  - When we don’t define any constructor in your class, compiler defines default one for us.
  - When we declare any constructor (in the example we have already defined a parameterized constructor), compiler doesn’t do it for you.
  - Since we have defined a constructor in class code, compiler didn’t create default one.
  - While creating object we are invoking default one, which doesn’t exist in class code. Then the code gives an compilation error.

<br/>
<div align="right">
  <b><a href="#dependency-injection-di">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Dependency Injection with XML Configuration (Setter Injection)

### Create Setter Methods in the Class for Injections

Create `CricketCoach` class and add the setter methods for injections:

```java
package com.luv2code.springdemo;

public class CricketCoach implements Coach {
    private FortuneService fortuneService;

    // create a no-arg constructor
    public CricketCoach() {
        System.out.println("CricketCoach: inside no-arg constructor");
    }

    // create setter methods for injections
    public void setFortuneService(FortuneService fortuneService) {
        System.out.println("CricketCoach: inside setter method - setFortuneService");
        this.fortuneService = fortuneService;
    }

    @Override
    public String getDailyWorkout() {
        return "Practice fast bowling for 15 minutes";
    }

    @Override
    public String getDailyFortune() {
        return fortuneService.getFortune();
    }
}
```

### Configure the Dependency Injection in Spring Config File

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- Define the dependency-->
    <bean id="myFortuneService" class="com.luv2code.springdemo.HappyFortuneService"></bean>

    <!-- Define your beans here -->
    <bean id="myCoach" class="com.luv2code.springdemo.TrackCoach">
        <!-- Set up constructor injection -->
        <constructor-arg ref="myFortuneService" />
    </bean>

    <bean id="myCricketCoach" class="com.luv2code.springdemo.CricketCoach">
        <!-- Set up setter injection -->
        <property name="fortuneService" ref="myfortuneService" />
    </bean>
</beans>
```

- The `id` is like an alias.
- The `class` should be the fully qualified class name of implementation class.
- The `name` equals `fortuneService` but the Spring Framework will actually call `setFortuneService`.

### Demo with Setter Injections

Create a new class named `SetterDemoApp`:

```java
package com.luv2code.springdemo;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class SetterDemoApp {
    public static void main(String[] args) {
        // load the spring configuration file
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        // retrieve bean from spring container
        CricketCoach theCoach = context.getBean("myCricketCoach", CricketCoach.class);

        // call methods on the bean
        System.out.println(theCoach.getDailyWorkout());
        System.out.println(theCoach.getDailyFortune());

        // close the context
        context.close();
    }
}
```

### Inject Literal Values

The steps for injecting literal values could are:

1. Create private fields for setting methods.

    ```java
    package com.luv2code.springdemo;

    public class CricketCoach implements Coach {
        // add new fields for email address and team
        private String emailAddress;
        private String team;

        // add methods for injections
        private FortuneService fortuneService;

        // create a no-arg constructor
        public CricketCoach() {
            System.out.println("CricketCoach: inside no-arg constructor");
        }

        // getter and setter for EmailAddress
        public String getEmailAddress() {
            return emailAddress;
        }

        public void setEmailAddress(String emailAddress) {
            System.out.println("CricketCoach: inside setter method - setEmailAddress");
            this.emailAddress = emailAddress;
        }

        // getter and setter for Team
        public String getTeam() {
            return team;
        }

        public void setTeam(String team) {
            System.out.println("CricketCoach: inside setter method - setTeam");
            this.team = team;
        }

        // create setter methods for injections
        public void setFortuneService(FortuneService fortuneService) {
            System.out.println("CricketCoach: inside setter method - setFortuneService");
            this.fortuneService = fortuneService;
        }

        @Override
        public String getDailyWorkout() {
            return "Practice fast bowling for 15 minutes";
        }

        @Override
        public String getDailyFortune() {
            return fortuneService.getFortune();
        }
    }
    ```

2. Use `<property>` to apply the `name` and `ref` for the values.

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
        <bean id="myCoach" class="com.luv2code.springdemo.TrackCoach">
            <!-- set up constructor injection -->
            <constructor-arg ref="myFortuneService" />
        </bean>

        <bean id="myCricketCoach" class="com.luv2code.springdemo.CricketCoach">
            <!-- set up setter injection -->
            <property name="fortuneService" ref="myFortuneService" />

            <!-- inject literal values -->
            <property name="emailAddress" value="thebestcoach@luv2code.com" />
            <property name="team" value="Sunrisers Hyderabad" />
        </bean>
    </beans>
    ```

### Inject Values from a Properties File

If we don't want to hard coded the literal values in the config file, we can read this information from the application file by following steps:

1. Create Properties File

    ```properties
    foo.email=myeasycoach@luv2code.com
    foo.team=Royal Challengers Bangalore
    ```

2. Load Properties File in Spring config File

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans" ... >
        ...
        <!-- load the properties file: sport.properties -->
        <context:property-placeholder location="classpath:sport.properties" />
        ...
    </beans>
    ```

3. Reference values from Properties File

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans" ... >
        ...
        <bean id="myCricketCoach" class="com.luv2code.springdemo.CricketCoach">
            ...
            <!-- inject literal values -->
            <property name="emailAddress" value="${foo.email}" />
            <property name="team" value="${foo.team}" />
        </bean>
        ...
    </beans>
    ```

<br/>
<div align="right">
  <b><a href="#dependency-injection-di">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Dependency Injection with Java Annotations (Constructor Injection)

### Define the Dependency Interface and Class

Create the `FortuneService` interface:

```java
package com.luv2code.springdemo;

public interface FortuneService {
    public String getFortune();
}
```

Then create the class `HappyFortuneService` to implement the `FortuneService` interface:

```java
package com.luv2code.springdemo;

import org.springframework.stereotype.Component;

@Component
public class HappyFortuneService implements FortuneService {
    @Override
    public String getFortune() {
        return "Today is your lucky day!";
    }
}
```

### Create Constructor in the Class for Injections and Add `@Autowired` Annotation

```java
package com.luv2code.springdemo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class TennisCoach implements Coach {
    private FortuneService fortuneService;

    @Autowired
    public TennisCoach(FortuneService theFortuneService) {
        fortuneService = theFortuneService;
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
  <b><a href="#dependency-injection-di">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Dependency Injection with Java Annotations (Setter Injection)

```java
package com.luv2code.springdemo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class TennisCoach implements Coach {
    private FortuneService fortuneService;

    // define a default constructor
    public TennisCoach() {
        System.out.println(">> TennisCoach: inside default constructor");
    }

    // define a setter method
    @Autowired
    public void setFortuneService(FortuneService theFortuneService) {
        System.out.println(">> TennisCoach: inside setFortuneService() method");
        fortuneService = theFortuneService;
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
  <b><a href="#dependency-injection-di">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Dependency Injection with Java Annotations (Field Injection)

```java
package com.luv2code.springdemo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class TennisCoach implements Coach {
    @Autowired
    private FortuneService fortuneService;

    // define a default constructor
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

<br/>
<div align="right">
  <b><a href="#dependency-injection-di">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Dependency Injection with Java Configuration Class

### Define Method on Expose Bean and Inject Bean Depencies

Add the `SwimCoach` class:

```java
package com.luv2code.springdemo;

public class SwimCoach implements Coach {
    private FortuneService fortuneService;

    public SwimCoach(FortuneService theFortuneService) {
        fortuneService = theFortuneService;
    }

    @Override
    public String getDailyWorkout() {
        return "Swim 1000 meters as a warm up.";
    }

    @Override
    public String getDailyFortune() {
        return fortuneService.getFortune();
    }
}
```

### Modify the Config File

```java
package com.luv2code.springdemo;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SportConfig {
    // define bean for our sad fortune service
    @Bean
    public FortuneService sadFortuneService() {
        return new SadFortuneSerivce();
    }

    // define bean for our swim coach and inject dependency
    @Bean
    public Coach swimCoach() {
        return new SwimCoach(sadFortuneService());
    }
}
```

### Behind the Scenes

For this code:

```java
@Bean
public Coach swimCoach() {
    SwimCoach mySwimCoach = new SwimCoach();
        return mySwimCoach;
}
```

1. The `@Bean` annotation tells Spring that we're creating a bean component manually. The default scope is **singleton** since we don't specify a scope.
2. The `public Coach swimCoach()` will specify the bean use bean id `swimCoach`.
   - The method name determines the bean id.
   - The return type is the `Coach` interface.
   - This helps Spring find any dependencies that implement the `Coach` interface.
3. The `@Bean` annotation will intercept any requests for `swimCoach` bean for **singleton** scope. As a result, it will give the same instance of the bean for any requests.

Behind the scenes, during the `@Bean` interception, it will check in memory of the Spring container (`applicationContext`) and see if this given bean has already been created.

### Inject Values from a Properties File

1. Add Properties File named `sport.properties`.

    ```
    foo.email=myeasycoach@luv2code.com
    foo.team=Awesome Java Coders
    ```

2. Load Properties File in Spring with `@PropertySource` Annotation

    ```java
    package com.luv2code.springdemo;

    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.context.annotation.PropertySource;

    @Configuration
    @PropertySource("classpath:sport.properties")
    public class SportConfig {
        // define bean for our sad fortune service
        @Bean
        public FortuneService sadFortuneService() {
            return new SadFortuneSerivce();
        }

        // define bean for our swim coach and inject dependency
        @Bean
        public Coach swimCoach() {
            return new SwimCoach(sadFortuneService());
        }
    }
    ```

3. Reference values from Properties File with `@Value` Annotation

    ```java
    package com.luv2code.springdemo;

    import org.springframework.beans.factory.annotation.Value;

    public class SwimCoach implements Coach {

        @Value("${foo.email}")
        private String email;

        @Value("${foo.team}")
        private String team;

        public String getEmail() {
            return email;
        }

        public String getTeam() {
            return team;
        }

        private FortuneService fortuneService;

        public SwimCoach(FortuneService theFortuneService) {
            fortuneService = theFortuneService;
        }

        @Override
        public String getDailyWorkout() {
            return "Swim 1000 meters as a warm up.";
        }

        @Override
        public String getDailyFortune() {
            return fortuneService.getFortune();
        }
    }
    ```

<br/>
<div align="right">
  <b><a href="#inversion-of-control-ioc">[ ↥ Back To Top ]</a></b>
</div>
<br/>