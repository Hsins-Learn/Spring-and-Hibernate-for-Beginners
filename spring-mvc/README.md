# Spring MVC

- [Overview](#overview)
  - [Model-View-Control (MVC)](#model-view-control-mvc)
  - [Spring MVC](#spring-mvc-1)
- [Spring MVC Configuration](#spring-mvc-configuration)
- [Controllers and Views](#controllers-and-views)
  - [Overview](#overview-1)
  - [Development Process](#development-process)
  - [Basic Controller](#basic-controller)
    - [Create Controller Class and Define Controller Method](#create-controller-class-and-define-controller-method)
    - [Develop View Page](#develop-view-page)
  - [Read HTML from Data](#read-html-from-data)
    - [Create Controller Class and Define Controller Method](#create-controller-class-and-define-controller-method-1)
    - [Add the View Pages](#add-the-view-pages)
  - [Add Data to the Spring Model](#add-data-to-the-spring-model)
    - [Passing Model to the Controller](#passing-model-to-the-controller)
    - [Add the View Pages](#add-the-view-pages-1)
- [Request Params and Request Mappings](#request-params-and-request-mappings)
  - [Basic Binding](#basic-binding)
  - [Controller Level Request Mapping](#controller-level-request-mapping)
- [Form Tags and Data Binding](#form-tags-and-data-binding)
  - [Overview](#overview-2)
  - [Text Fields](#text-fields)
    - [Create Controller and Add Attributes](#create-controller-and-add-attributes)
    - [Add Template View](#add-template-view)
  - [Drop-Down Lists](#drop-down-lists)
    - [Hard Code: Update View](#hard-code-update-view)
    - [Options Refer to the Collection of Data](#options-refer-to-the-collection-of-data)
  - [Radio Buttons](#radio-buttons)
  - [Checkboxes](#checkboxes)
- [Form Validation](#form-validation)
  - [Overview](#overview-3)
  - [Setup Development Environment](#setup-development-environment)
    - [Version Problem](#version-problem)
    - [Install Validation Files](#install-validation-files)
  - [Check for Required Fields](#check-for-required-fields)
    - [Development Process](#development-process-1)
    - [Add Validation Rules](#add-validation-rules)
    - [Display Error Messages](#display-error-messages)
    - [Perform Validation in the Controller Class](#perform-validation-in-the-controller-class)
    - [Add Pre-Processing Code with `@InitBinder`](#add-pre-processing-code-with-initbinder)
  - [Validate with Number Ranges and Regular Expression](#validate-with-number-ranges-and-regular-expression)
    - [Add Validation Rule to Class](#add-validation-rule-to-class)
    - [Display Error Messages](#display-error-messages-1)
  - [Custom Error Messages](#custom-error-messages)
    - [Development Process](#development-process-2)
    - [Create Custom Error Message](#create-custom-error-message)
    - [Load Resource](#load-resource)
  - [Custom Validation Rules](#custom-validation-rules)
    - [Development Process](#development-process-3)
    - [Create `@CourseCode` Annotation](#create-coursecode-annotation)
    - [Develop the ConstraintValidator](#develop-the-constraintvalidator)
    - [Add Validation Rule to Class](#add-validation-rule-to-class-1)

## Overview

### Model-View-Control (MVC)

The **Model-View-Controller (MVC)** is an architectural pattern that separates an application into three main logical components: the **Model**, the **View**, and the **Controller**. Each of these components are built to handle specific development aspects of an application.

- **Model** corresponds to all the data-related logic that the user works with.
- **View** is used for all the UI logic of the application.
- **Controller** act as an interface between Model and View components to process all the business logic and incoming requests, manipulate data using the Model component and interact with the Views to render the final output.

### Spring MVC

**Spring MVC** is a framework based on Model-View-Controller (MVC) design pattern for building web applications in Java. It can leverages features of the Core Spring Framework (IoC, DI).

<div align="center">
  <img src="https://i.imgur.com/M1zsDUz.png" alt="Model-View-Control (MVC)">
</div>

The figure shows the Spring MVC framework components:

- **Front Controller**
  - Part of the Spring Framework and known as `DispatcherServlet`.
  - Already developed by Spring Dev Team.
- **Controller**
  - Code created by developers (us).
  - Contains the business logic. e.g. handling the requests, storing/retrieving data, placing data in models
  - Send to appropriate view template.
- **Model**
  - Contains our data.
  - Store/retreieve data via backend systems (database, web service, etc...).
  - Data can be any Java object/collection.
- **View Template**
  - Spring MVC is flexible and supports many view templates.
  - The most common is **JSP + JSTL**.
  - Developers create the pages for displaying data.
  - Other templates supported: Thymeleaf, Groovy, Velocity, Freemarker, etc...

<br/>
<div align="right">
  <b><a href="#spring-mvc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Spring MVC Configuration

1. Add configurations to file: `WEB-INF/web.xml`
     - Configure Spring MVC Dispatcher Servlet
     - Setup URL mapping to Spring MVC Dispatcher Servlet
2. Add configurations to file: `WEB-INF/spring-mvc-demo-servlet.xml`
     - Add support for Spring component scanning.
     - Add support for conversion, formatting and validation.
     - Configure Spring MVC View Resolver.

Check the [Setup Development Environment: Spring MVC Project](./../setup-dev-environment/README.md#spring-mvc-project-spring-mvc-demo) for more information.

<br/>
<div align="right">
  <b><a href="#spring-mvc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Controllers and Views

### Overview

What we're gonna do here is actually setting up a request mapping for a given path. This request path should be sent to the Controller for forwarding it over to a view template:

- **Request Path**: `/`.
- **Controller**: `HomeController`.
- **View Template**: `main-menu.jsp`.

### Development Process

1. Create Controller Class.
2. Define Controller Method.
3. Add Request Mapping to Controller Method.
4. Return View Name.
5. Develop View Page.

### Basic Controller

#### Create Controller Class and Define Controller Method

Let's create a new package named `com.luv2code.springdemo.mvc` and create the Controller class `HomeController`:

```java
@Controller
public class HomeController {
    @RequestMapping("/")
    public String showPage() {
        return "main-menu";
    }
}
```

- Annotate class with `@Controller` annotation which inherits from `@Component` supports scanning.
- Spring MVC is flexible and we can use any method name.
- Use `@RequestMapping` annotation for adding request mapping.
- Spring will use the information from the configuration file for finding the view page.
    - Example Configuration File.
        ```xml
        <bean
          class="org.springframework.web.servlet.view.InternalResourceViewResolver">
          <property name="prefix" value="/WEB-INF/view/" />
          <property name="suffix" value=".jsp" />
        </bean>
        ```
    - How Spring find the Template View?
        ```
         /WEB-INF/view/  main-menu  .jsp
        [‾‾‾‾‾‾‾‾‾‾‾‾‾‾][‾‾‾‾‾‾‾‾‾][‾‾‾‾]
         prefix          view name  suffix
        ```

#### Develop View Page

Add `main-menu.jsp` file inside the `/WEB-INF/view/` folder:

```html
<!DOCTYPE html>
<html>
  <body>
    <h2>Spring MVC Demo - Home Page</h2>
  </body>
</html>
```

Now, right click the project name and choose [Run As] > [Run on Server].

### Read HTML from Data

#### Create Controller Class and Define Controller Method

We're going to define two methods inside the `HelloWorldController`:

```java
@Controller
public class HelloWorldController {
    // need a controller method to show the initial HTML form
    @RequestMapping("/showForm")
    public String showForm() {
        return "helloworld-form";
    }

    // need a controller method to process the HTML form
    @RequestMapping("/processForm")
    public String processForm() {
        return "helloworld";
    }
}
```

#### Add the View Pages

Add `helloworld-form` file inside the `/WEB-INF/view/` folder:

```jsp
<!DOCTYPE html>
<html>
  <head>
    <title>Hello World - Input Form</title>
  </head>
  <body>
    <form action="processForm" method="GET">
      <input type="text" name="studentName" placeholder="What's your name?" />
      <input type="submit" />
    </form>
  </body>
</html>
```

Add `helloworld` file inside the `/WEB-INF/view/` folder:

```jsp
<!DOCTYPE html>
<html>
  <body>
    <h2>Hello World of Spring!</h2>
    <div>
      Student name: ${param.studentName}
    </div>
  </body>
</html>
```

### Add Data to the Spring Model

#### Passing Model to the Controller

The **Model** is a container for our application data. We can put anything in the model in the Controller.

```java
@Controller
public class HelloWorldController {
    @RequestMapping("/processFormVersionTwo")
    public String letsShoutDude(HttpServletRequest request, Model model) {
        // read the request parameter from the HTML form
        String theName = request.getParameter("studentName");

        // convert the data to all caps
        theName = theName.toUpperCase();

        // create the message
        String result = "Yo!" + theName;

        // add message to the model
        model.addAttribute("message", result);

        return "helloworld";
    }
}
```

#### Add the View Pages

Now, the view template can get data with given attribute name from the model:

```jsp
<!DOCTYPE html>
<html>
  <body>
    <h2>Hello World of Spring!</h2>
    <div>
      Student name: ${param.studentName}
      <br><br>
      The message: ${message}
    </div>
  </body>
</html>
```

<br/>
<div align="right">
  <b><a href="#spring-mvc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Request Params and Request Mappings

### Basic Binding

Spring has a special annotation called `RequestParam` which allow us to read form data and binds it to a parameter automatically.

```java
@Controller
public class HelloWorldController {
    @RequestMapping("/processFormVersionThree")
    public String processFormVersionThree(@RequestParam("studentName") String theName, Model model) {
        // convert the data to all caps
        theName = theName.toUpperCase();

        // create the message
        String result = "Hey My Friend from v3! " + theName;

        // add message to the model
        model.addAttribute("message", result);

        return "helloworld";
    }
}
```

### Controller Level Request Mapping

We can also add `@RequestMapping` annotation to a controller. It serves as parent mapping for controller and all the request mappings on methods in the controller are relative.

```java
@Controller
@RequestMapping("/hello")
public class HelloWorldController {
    // need a controller method to show the initial HTML form
    @RequestMapping("/showForm")
    public String showForm() {
        return "helloworld-form";
    }
}
```

<br/>
<div align="right">
  <b><a href="#spring-mvc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Form Tags and Data Binding

### Overview

Spring MVC **Form Tags** are the building block for a webpage and these Form Tags are configurable and resuable. The Form Tags can make use of data binding and automatically set / retrieve data from a Java bean.

|     Form Tags      | Description           |
| :----------------: | :-------------------- |
|    `form:form`     | main form container   |
|    `form:input`    | text field            |
|  `form:textarea`   | multi-line text field |
|  `form:checkbox`   | check box             |
| `form:radiobutton` | radio buttons         |
|   `form:select`    | drop-down list        |
|     more ....      |                       |

Just specify the Spring namespace at beginning of JSP file for referencing Spring MVC Form Tags:

```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
```

### Text Fields

#### Create Controller and Add Attributes

Before we shown the form, we must add model attributes. These are the beans that will hold form data for data binding. Let's create the `StudentController` class:

```java
@Controller
@RequestMapping("/student")
public class StudentController {
    @RequestMapping("/showForm")
    public String showForm(Model theModel) {
        // create a student object
        Student theStudent = new Student();

        // add student object to the model
        theModel.addAttribute("student", theStudent);

        return "student-form";
    }

    @RequestMapping("/processForm")
    public String processForm(@ModelAttribute("student") Student theStudent) {
        // log the input data
        System.out.println("theStudent: " + theStudent.getFirstName() + " " + theStudent.getLastName());

        return "student-confirmation";
    }
}
```

#### Add Template View

Add the form view `student-form.jsp` file:

```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>

<!DOCTYPE html>
<html>
  <head>
    <title>Student Registration Form</title>
  </head>
  <body>
    <form:form action="processForm" modelAttribute="student">
      First name: <form:input path="firstName" />
      <br><br>
      Last name: <form:input path="lastName" />
      <br><br>
      <input type="submit" value="submit" />
    </form:form>
  </body>
</html>
```

Add the form view `student-confirmation.jsp` file:

```jsp
<!DOCTYPE html>
<html>
  <head>
    <title>Student Confirmation</title>
  </head>
  <body>
    The student is confirmed: ${student.firstName} ${student.lastName}
  </body>
</html>
```

Note that the Spring will call `student.getFirstName()` for `${student.firstName}` inside the view.

### Drop-Down Lists

#### Hard Code: Update View

```jsp
<html>
  <body>
    <form:form action="processForm" modelAttribute="student">
      ...
      Country:
      <form:select path="country">
        <form:option value="Brazil" label="Brazil" />
        <form:option value="France" label="France" />
        <form:option value="Germany" label="Germany" />
        <form:option value="India" label="India" />
      </form:select>
      ...
    </form:form>
  </body>
</html>
```

#### Options Refer to the Collection of Data

Let's add the `getCountryOptions()` for generating collection of countrys:

```java
package com.luv2code.springdemo.mvc;

import java.util.LinkedHashMap;

public class Student {
    private String firstName;
    private String lastName;

    private String country;

    private LinkedHashMap<String, String> countryOptions;

    public Student() {
        // populate country options: used ISO country code
        countryOptions = new LinkedHashMap<>();
        countryOptions.put("BR", "Brazil");
        countryOptions.put("FR", "France");
        countryOptions.put("DE", "Germany");
        countryOptions.put("IN", "India");
        countryOptions.put("US", "United States of America");
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getCountry() {
        return country;
    }
    public void setCountry(String country) {
        this.country = country;
    }

    public LinkedHashMap<String, String> getCountryOptions() {
        return countryOptions;
    }
}
```

Then we can use `<form:options items="${student.countryOptions}" />` for getting data from that method:

```jsp
<html>
  <body>
    <form:form action="processForm" modelAttribute="student">
      ...
      Country:
      <form:select path="country">
        <form:options items="${student.countryOptions}" />
      </form:select>
      ...
    </form:form>
  </body>
</html>
```

### Radio Buttons

Add following code to `student-form.jsp`:

```jsp
<html>
  <body>
    <form:form action="processForm" modelAttribute="student">
      ...
      Favorite Languages:
      Java <form:radiobutton path="favoriteLanguage" value="Java" />
      C# <form:radiobutton path="favoriteLanguage" value="C#" />
      PHP <form:radiobutton path="favoriteLanguage" value="PHP" />
      Ruby <form:radiobutton path="favoriteLanguage" value="Ruby" />
      ...
    </form:form>
  </body>
</html>
```

### Checkboxes

Add following code to `student-form.jsp`:

```jsp
<html>
  <body>
    <form:form action="processForm" modelAttribute="student">
      ...
      Operation Systems:
      Linux <form:checkbox path="operatingSystems" value="Linux" />
      macOS <form:checkbox path="operatingSystems" value="macOS" />
      Windows <form:checkbox path="operatingSystems" value="Windows" />
      ...
    </form:form>
  </body>
</html>
```

Add following code to `student-confirmation.jsp`:

```jsp
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<!DOCTYPE html>
<html>
  <body>
    ...
    Operating Systems:
    <ul>
      <c:forEach var="temp" items="${student.operatingSystems}">
        <li>${temp}</li>
      </c:forEach>
    </ul>
    ...
  </body>
</html>
```

Refer to [Unknown tag (c:foreach). In eclipse](https://stackoverflow.com/questions/22708241/unknown-tag-cforeach-in-eclipse), just add `<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>` in front of `student-confirmation.jsp` for the issue "Unkown tag... <c:forEach... />".

<br/>
<div align="right">
  <b><a href="#spring-mvc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Form Validation

### Overview

Java has a standard Bean Validation API which defines a metadata model and API for entity validation:

|    Annotation     | Description                                  |
| :---------------: | :------------------------------------------- |
|    `@NotNull`     | checks that the annotated value is not null  |
|      `@Min`       | must be a number greather than value         |
|      `@Max`       | must be a number less than value             |
|      `@Size`      | size must match the given size               |
|    `@Pattern`     | must match a regular expression pattern      |
| `@Future`/`@Past` | date must be in future or past of given data |
|     others...     |                                              |

- Spring 4 and higher supports Bean Validation API.
- Preferred for validation when building Spring applications.

### Setup Development Environment

#### Version Problem

The Java's standard Bean Validation API (JSR-303) is only a specification and we need a implementation for it. The Hibernate Team have a fully compliant JSR-303 implementation: [Hibernate Validator](https://hibernate.org/validator/). There are something about versions with Hibernate Validator:

- The Jakarta EE is the community version of Java EE (rebranded, relicensed). And Jakarta EE doesn't replace Java EE.
    - Last version is Java EE 8 (August 2017).
    - Jakarta EE is moving forward with Jakarta EE 9 (December 2020) and future releases.
    - At the moment, main change with Jakarta EE is the package naming: `javax.*` > `jakarta.*`.
- Hibernate Validator 7 is based on **Jakarta EE 9**.
- Spring 5 is still based on some components of Java EE (`javax.*`).

As a result, Spring 5 is not compatible with Hibernate Validator 7. And there are two releases of Hibernate Validator with the same features:

- **Hibernate Validator 7** is for Jakarta EE 9 project.
- **Hibernate Validator 6.2** is compatible with Spring 5.

#### Install Validation Files

1. Visit [Hibernate Validator](https://hibernate.org/validator/) website.
2. Navigate to [Release] > [6.2] and click [Download the Zip archive].
3. Unzip the file and copy the `*.jar` in `./dist` and `./lib/required` folders to the project's `/WebContent/WEB-INF/lib` directory.

<br/>
<div align="right">
  <b><a href="#spring-mvc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

### Check for Required Fields

#### Development Process

1. Add Validation Rule to Customer Class.
2. Display Error Messages on HTML Form.
3. Perform Validation in the Controller Class.
4. Update Confirmation Page.

#### Add Validation Rules

```java
public class Customer {
    private String firstName;

    @NotNull(message = "is required")
    @Size(min = 1, message = "is required")
    private String lastName;

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
}
```

#### Display Error Messages

Create `customer-form.jsp` for displaying error messages:

```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>

<!DOCTYPE html>
<html>
  <head>
    <title>Customer Registration Form</title>
    <style>
      .error {
        color: red;
      }
    </style>
  </head>
  <body>
    <i>Fill out the form. Asterisk (*) means required.</i>
    <br>
    <form:form action="processForm" modelAttribute="customer">
	    First name: <form:input path="firstName" />
	    <br>
	    Last name (*): <form:input path="lastName" />
	    <form:errors path="lastName" cssClass="error" />
	    <br>
	    <input type="submit" value="Submit" />
	  </form:form>
  </body>
</html>
```

#### Perform Validation in the Controller Class

```java
@Controller
@RequestMapping("/customer")
public class CustomerController {
    @RequestMapping("/showForm")
    public String showForm(Model theModel) {
        theModel.addAttribute("customer", new Customer());
        return "customer-form";
    }

    @RequestMapping("/processForm")
    public String processForm(@Valid @ModelAttribute("customer") Customer theCustomer, BindingResult theBindingResult) {
        if (theBindingResult.hasErrors()) {
            return "customer-form";
        } else {
            return "customer-confirmation";
        }
    }
}
```

#### Add Pre-Processing Code with `@InitBinder`

The `@InitBinder` annotation works as a pre-processor, and it will pre-process each web request to our controller.

```java
@Controller
@RequestMapping("/customer")
public class CustomerController {
    // add an initBinder to convert trim input strings
    // remove leading and trailing whitespace
    @InitBinder
    public void initBinder(WebDataBinder dataBinder) {
        StringTrimmerEditor stringTrimmerEditor = new StringTrimmerEditor(true);
        dataBinder.registerCustomEditor(String.class, stringTrimmerEditor);
    }
    ...
}
```

We've use `@InitBinder` to trim Strings (removing leading and trailing white space).

<br/>
<div align="right">
  <b><a href="#spring-mvc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

### Validate with Number Ranges and Regular Expression

#### Add Validation Rule to Class

```java
public class Customer {
    ...
    // Number Range Validation Rule
    @Min(value=0, message="must be greater than or equal to zero")
    @Max(value=10, message="must be less than or equal to 10")
    private int freePasses;
    ...
    // Regular Expression Validation Rule
    @Pattern(regexp = "^[a-zA-Z0-9]{5}", message = "only 5 chars/digits")
    private String postalCode;
    ...
}
```

#### Display Error Messages

```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>

<!DOCTYPE html>
<html>
  <body>
    <form:form action="processForm" modelAttribute="customer">
      ...
      Free passes: <form:input path="freePasses" />
      <form:errors path="freePasses" cssClass="error" />
      Postal Code: <form:input path="postalCode" />
      <form:errors path="postalCode" cssClass="error" />
      ...
	  </form:form>
  </body>
</html>
```

<br/>
<div align="right">
  <b><a href="#spring-mvc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

### Custom Error Messages

#### Development Process

1. Create Custom Error Message: `src/resources/messages.properties`.
2. Load Custom Messages Resource in Spring Config File: `WebContent/WEB-INF/spring-mvc-demo-servlet.xml`.

#### Create Custom Error Message

```
 typeMismatch . customer . freePasses = Invalid number
[‾‾‾‾‾‾‾‾‾‾‾‾] [‾‾‾‾‾‾‾‾] [‾‾‾‾‾‾‾‾‾‾] [‾‾‾‾‾‾‾‾‾‾‾‾‾‾]
 Error Type     Attribute  Field Name   Value
```

#### Load Resource

Add following bean property in `spring-mvc-demo-servlet.xml`:

```xml
<beans ...>
  ...
  <!-- Load custom message resources -->
  <bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
    <property name="basenames" value="resources/messages" />
  </bean>
  ...
</beans>
```

<br/>
<div align="right">
  <b><a href="#spring-mvc">[ ↥ Back To Top ]</a></b>
</div>
<br/>

### Custom Validation Rules

#### Development Process

For custom validation, we will create a **Custom Java Annotation** by following steps:

1. Create Custom Validation Rule.
2. Add Validation Rule to Class.
3. Display Error Messages on HTML Form.
4. Update Confirmation Page.

#### Create `@CourseCode` Annotation

Create a new package `com.luv2code.springdemo.mvc.validation` and add an annotation named `CourseCode`:

```java
@Constraint(validatedBy = CourseCodeConstraintValidator.class)
@Target({ ElementType.METHOD, ElementType.FIELD })
@Retention(RetentionPolicy.RUNTIME)

public @interface CourseCode {
    // define default course code
    public String value() default "LUV";

    // define default error message
    public String message() default "must start with LUV";
}
```

#### Develop the ConstraintValidator

The `CourseCodeConstraintValidator` class should be a helper class that contains business rules / validation logic:

```java
public class CourseCodeConstraintValidator implements ConstraintValidator<CourseCode, String> {
    private String coursePrefix;

    @Override
    public void initialize(CourseCode theCourseCode) {
        coursePrefix = theCourseCode.value();
    }

    @Override
    public boolean isValid(String theCode, ConstraintValidatorContext theConstraintValidatorContext) {
        boolean result;

        if (theCode != null) {
            result = theCode.startsWith(coursePrefix);
        } else {
            result = true;
        }
        return result;
    }
}
```

#### Add Validation Rule to Class

```java
public class Customer {
    ...
    @CourseCode(value = "TOPS", message = "must start with TOPS")
    private String courseCode;
    ...
}
```

<br/>
<div align="right">
  <b><a href="#spring-mvc">[ ↥ Back To Top ]</a></b>
</div>
<br/>