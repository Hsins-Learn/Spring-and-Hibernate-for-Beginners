# Maven

- [Maven](#maven)
  - [Overview](#overview)
    - [Maven](#maven-1)
    - [Standard Directory Structure](#standard-directory-structure)
  - [Maven Key Concepts](#maven-key-concepts)
    - [POM File: `pom.xml`](#pom-file-pomxml)
    - [Project Coordinates and Dependency Coordinates](#project-coordinates-and-dependency-coordinates)
  - [Maven Archetypes](#maven-archetypes)

## Overview

### Maven

**Maven** is a Project Management tool and most popular use of Maven is for build management and depencies.

- We need additional JAR files when building the Java project.
  - Without Maven: download and add all of them manually.
  - With Maven: Maven will go and download the JAR files for us. And then make those files avaiable during compile/run.
- How does Maven work?
  1. Read the config file.
  2. Check local repository (Maven Local Repository).
  3. Get from remote repository (Maven Central Repository).
  4. Save files to the local repository.
  5. Build and run.
- When Maven retrieves the project dependency, it will also download supporting dependencies.

### Standard Directory Structure

Maven provides a standard directory structure shown like below:

```
├── pom.xml
├── src
│   ├── main
│   │   ├── java        // our Java source cde
│   │   │   └─ ...
│   │   ├── resoource   // properties / config files ued by our app
│   │   │   └─ ...
│   │   └── webapp      // JSP files, web config files, and other web assets (images, css, js, etc)
│   │       └─ ...
│   └── test            // unit testing code and properties
│       ├── java
│       │   └─ ...
│       └── resoource
│           └─ ...
├── target              // destination directory for compiled code. automatically created by Maven
└── ...
```

<br/>
<div align="right">
  <b><a href="#maven">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Maven Key Concepts

### POM File: `pom.xml`

```xml
<project ...>
    <modelVersion>4.0.0</modelVersion>

    <!-- Project Metadata -->
    <groupId>com.luv2code</groupId>
    <artifactId>mycoolapp</artifactId>
    <version>1.0.FINAL</version>
    <packaging>jar</packaging>

    <name>mycoolapp</name>

    <!-- Dependencies -->
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <!-- add plugins for customization -->
</project>
```

The **Project Object Model (POM) file** located in the root of Maven project directory is the configuration file for our project. It can be thought as the "shopping list" for dependencies.

The POM File Structure contains:

- **Project Metadata**: project name, version and output file type: JAR, WAR...
- **Dependencies**: lists of projects we depend on, e.g. Spring, Hibernates, etc...
- **Plugins**: additional custom tasks to run, e.g. generate JUnit test resports etc...

### Project Coordinates and Dependency Coordinates

The `<groudId>`, `<artifactId>` and `<version>` in Project Metadata are so-called **Project Coordinates**, which uniqutely identify a project. The Project Coordinates are similar to GPS coordinates which provide precise information for finding the building.

The elements in **Project Coordinates** are:

|     Name     | Description                                                                                                 |
| :----------: | :---------------------------------------------------------------------------------------------------------- |
|  `groupId`   | Name of company, group or organization. The convention is to use reverse domain name, e.g. `com.luv2code`.  |
| `artifactId` | Name for the project, e.g. `mycoolapp`.                                                                     |
|  `version`   | A specific release version like `1.0`, `1.6`... If project is under active development then `1.0-SNAPSHOP`. |

To add given dependency project, we also need `<groupId>`, `<artifactId>` and `<version>`:

- Also referred to as **GAV** (**G**roup ID, **A**rtifact ID and **V**ersion).
- The `<version>` is optional but it's a best practice to include it for DevOps.

There are multiple site where we can find these **Dependency Coordinates** from:

- The Project Page. e.g. [Hibernate](https://hibernate.org/) etc.
- [Maven Central Repository Search](https://search.maven.org/)
- [MVN Repository](https://mvnrepository.com/)

<br/>
<div align="right">
  <b><a href="#maven">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Maven Archetypes

The **Maven Archetypes** can be used to create new Maven projects. It contains templates files for a given Maven project. Just think of it as a collection of "starter files" for a project.

Maven provides serveral archetypes artifacts listed on [Maven Archetypes](https://maven.apache.org/archetypes/). We can use these archetypes for creating project with maven command line or IDEs.

- **Maven Command Line**: `mvn archetype:generate`
- **Eclipse**: [File] > [New Maven Project]

<br/>
<div align="right">
  <b><a href="#maven">[ ↥ Back To Top ]</a></b>
</div>
<br/>