# Setup Development Environment

- [Overview](#overview)
- [Apache Tomcat](#apache-tomcat)
- [Eclipse IDE](#eclipse-ide)
- [MySQL](#mysql)
- [First Project (`spring-demo-one`)](#first-project-spring-demo-one)
  - [Download Spring 5 JAR Files](#download-spring-5-jar-files)
  - [Add JAR Files to Java Build Path](#add-jar-files-to-java-build-path)
- [Spring MVC Project (`spring-mvc-demo`)](#spring-mvc-project-spring-mvc-demo)
  - [Create Project and Change Perspective](#create-project-and-change-perspective)
  - [Download and Add Spring JAR Files.](#download-and-add-spring-jar-files)
  - [Setup Config Files](#setup-config-files)
- [Hibernate Project (`hibernate-tutorial`)](#hibernate-project-hibernate-tutorial)
  - [Download Hibernate Files and MySQL JDBC](#download-hibernate-files-and-mysql-jdbc)
  - [Add JAR Files to Java Build Path](#add-jar-files-to-java-build-path-1)
  - [Testing JDBC Connection](#testing-jdbc-connection)
  - [Add Hibernate Configuration](#add-hibernate-configuration)

## Overview

- Must have the Java Development Kit (JDK) installed.
- Java Application Server: Tomcat
- Java Integrated Development Environment (IDE): Eclipse
- Database Server: MySQL

<br/>
<div align="right">
  <b><a href="#overview#setup-development-environment">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Apache Tomcat

- Tomcat 10 was released to suppoert Jakarta EE 9. The breaking change for Java EE apps is that it renamed packages `javax.*` to `jakarta.*`.
- Spring 5 currently doesn't support the new package renaming Jakarta EE 9.
- Use **Tomcat 9** for Spring 5 Applications. (Check and track the [issues #25354](https://github.com/spring-projects/spring-framework/issues/25354))
- Download and install **Tomcat 9** from [Tomcat 9 Software Downloads](https://tomcat.apache.org/download-90.cgi).
- After installation, visit [`http://localhost:8080`](http://localhost:8080) for verification.
- We can start/stop the Tomcat server from services manager in Windows.

<br/>
<div align="right">
  <b><a href="#overview#setup-development-environment">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Eclipse IDE

- Download and install **Eclipse IDE for Java EE Developers** from [Eclipse IDE Packages](https://www.eclipse.org/downloads/packages/).
- Connect Eclipse and Tomcat:
  1. Move down to the bottom section and click the "Servers" tab.
  2. Click "No servers are available. Click this link to create a new server...".
  3. Select "Tomcat v9.0 Server" and click "Next" button.
  4. Select the Tomcat Installation folder e.g. "C:\Program Files\Apache Software Foundation\Tomcat 9.0".
  5. Click "Next" and then "Finish" button.
  6. Now, there is "Tomcat v9.0 Server at localhost" shown in the "Servers" tab.
- Setup Perspective by clicking [Winodws] -> [Perspective] -> [Open Perspective] -> [Java].

<br/>
<div align="right">
  <b><a href="#overview#setup-development-environment">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## MySQL

- Download and install **MySQL** from [MySQL Community Server Downloads](https://dev.mysql.com/downloads/mysql/).
    1. Keep the "Developer Default" and click [Next].
    2. Check requirements and click [Next].
    3. Download and then click [Execute] to install products.
    4. Keep network settings as default.
- Setup database table by loading and running the SQL files.
    1. [[FILE](./mysql/create-user.sql)] Create a new MySQL user for our application.
    2. [[FILE](./mysql/student-tracker.sql)] Create a new database `student`.

<br/>
<div align="right">
  <b><a href="#overview#setup-development-environment">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## First Project (`spring-demo-one`)

### Download Spring 5 JAR Files

1. Create project by clicking [File] -> [New] -> [Java Project].
    - Project name: `spring-demo-one`.
    - Click "Don't Create" in the dialog for creating `module-info.java`.
2. Add library folder named `lib` by right clicking [`spring-demo-one`] (Project Name) -> [New] -> [Folder].
3. Visit [Spring Download](http://luv2code.com/downloadspring) and choose [Artifactory] -> [Artifacts].
4. Browse the path `libs-release > org > springframework > spring`.
5. Download the lastest version of `spring-5.x.x-dist.zip`.
6. Unzip and copy all the `/lib/*.jar` files to the project's `/lib` directory.

### Add JAR Files to Java Build Path

1. Edit project properties by right clicking [`spring-demo-one`] (Project Name) -> [Properties].
2. Select [Java Build Path] and click the [Libraries] tab.
3. Select [Classpath] and click [Add JARs].
4. Select all the `*.jar` files in the `/lib` directory.
5. Click [Close and Apploy].

<br/>
<div align="right">
  <b><a href="#overview#setup-development-environment">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Spring MVC Project (`spring-mvc-demo`)

### Create Project and Change Perspective

1. Change Perspective by clicking [Winodws] -> [Perspective] -> [Open Perspective] -> [Other]
    - Select [Java EE (default)] and click [Open].
2. Create project by clicking [File] -> [New] -> [Dynamic Web Project].
    - Project name: `spring-mvc-demo`.
    - Dynamic web module version: `3.0`.
    - Remove the default `src/main/java` and add folder named `src`.
    - Content directory: `WebContent`.

### Download and Add Spring JAR Files.

1. Download the lastest version of `spring-5.x.x-dist.zip` from [Spring Download](http://luv2code.com/downloadspring).
2. Unzip and copy all the `/lib/*.jar` files to the project's `WebContent/WEB-INF/lib` directory.
3. Copy [`javax.servlet.jsp.jstl-1.2.1.jar`](./spring-mvc-demo/lib/javax.servlet.jsp.jstl-1.2.1.jar) and [`javax.servlet.jsp.jstl-api-1.2.1.jar`](./spring-mvc-demo/lib/javax.servlet.jsp.jstl-api-1.2.1.jar) to the project's `WebContent/WEB-INF/lib` directory.

### Setup Config Files

1. Copy [spring-mvc-demo-servlet.xml](./spring-mvc-demo/config/spring-mvc-demo-servlet.xml) and [web.xml](./spring-mvc-demo/config/web.xml) to the project's `WebContent/WEB-INF` directory.
2. Create the `WebContent/WEB-INF/view/` folder since we define the Spring MVC view resolver with `<property name="prefix" value="/WEB-INF/view/" />`.

## Hibernate Project (`hibernate-tutorial`)

### Download Hibernate Files and MySQL JDBC

1. Create project by clicking [File] -> [New] -> [Java Project].
    - Project name: `hibernate-tutorial`.
    - Click "Don't Create" in the dialog for creating `module-info.java`.
2. Add library folder named `lib` by right clicking [`hibernate-tutorial`] (Project Name) -> [New] -> [Folder].
3. Download the latest version of Hibernate from [Hibernate ORM](https://hibernate.org/orm/)
4. Unzip and copy all the `/lib/required/*.jar` files to the project's `/lib` directory.
5. Download the MySQL JDBC Driver from [MySQL Download Connector/J](https://dev.mysql.com/downloads/connector/j/).
    - Select Operating System with "Platform Independent".
    - Download the ZIP archive file.
6. Unzip and copy the `mysql-connector-java-*-bin.jar` files to the project's `/lib` directory.

### Add JAR Files to Java Build Path

1. Edit project properties by right clicking [`spring-demo-one`] (Project Name) -> [Properties].
2. Select [Java Build Path] and click the [Libraries] tab.
3. Click [Add JARs].
4. Select all the `*.jar` files in the `/lib` directory.
5. Click [Close and Apploy].

### Testing JDBC Connection

1. Create new package named `com.luv2code.jdbc`.
2. Create new class named `TestJdbc` with following code under the package.
3. Run for testing JDBC connection.

```java
package com.luv2code.jdbc;

public class TestJdbc {
    public static void main(String[] args) {
        String jdbcUrl = "jdbc:mysql://localhost:3306/hb_student_tracker?useSSL=false&serverTimezone=UTC";
        String user = "hbstudent";
        String pass = "hbstudent";

        try {
            System.out.println("Connecting to database: " + jdbcUrl);
            Connection myConn = DriverManager.getConnection(jdbcUrl, user, pass);
            System.out.println("Connection successful!!!");
        } catch (Exception exc) {
            exc.printStackTrace();
        }
    }
}
```

### Add Hibernate Configuration

We can also setup the connection information in the Hibernate config file `hibernate.cfg.xml`:

```xml
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>

    <session-factory>

        <!-- JDBC Database connection settings -->
        <property name="connection.driver_class">com.mysql.cj.jdbc.Driver</property>
        <property name="connection.url">jdbc:mysql://localhost:3306/hb_student_tracker?useSSL=false&amp;serverTimezone=UTC</property>
        <property name="connection.username">hbstudent</property>
        <property name="connection.password">hbstudent</property>

        <!-- JDBC connection pool settings ... using built-in test pool -->
        <property name="connection.pool_size">1</property>

        <!-- Select our SQL dialect -->
        <property name="dialect">org.hibernate.dialect.MySQLDialect</property>

        <!-- Echo the SQL to stdout -->
        <property name="show_sql">true</property>

		<!-- Set the current session context -->
		<property name="current_session_context_class">thread</property>

    </session-factory>

</hibernate-configuration>
```

<br/>
<div align="right">
  <b><a href="#overview#setup-development-environment">[ ↥ Back To Top ]</a></b>
</div>
<br/>