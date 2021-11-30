# Hibernate

- [Hibernate](#hibernate)
  - [Overview](#overview)
    - [Hibernate Framework](#hibernate-framework)
    - [Object-To-Relational Mapping (ORM)](#object-to-relational-mapping-orm)
    - [Entity Lifecycle](#entity-lifecycle)
  - [Hibernate Annotations](#hibernate-annotations)
    - [Development Process](#development-process)
    - [Mapping Class and Fields](#mapping-class-and-fields)
  - [CRUD: Create, Read, Update and Delete](#crud-create-read-update-and-delete)
    - [Two Key Players](#two-key-players)
    - [Create and Save Java Objects](#create-and-save-java-objects)
    - [Primary Key](#primary-key)
    - [Retrieve Java Object with Hibernate](#retrieve-java-object-with-hibernate)
    - [Query Objects](#query-objects)
    - [Update Objects](#update-objects)
    - [Delete Objects](#delete-objects)
  - [Advanced Mapping](#advanced-mapping)
    - [Relationships Between Tables](#relationships-between-tables)
    - [Primary Key and Foreign Key](#primary-key-and-foreign-key)
    - [Cascade](#cascade)
    - [Fetch Types: Eager vs Lazy Loading](#fetch-types-eager-vs-lazy-loading)
  - [Mapping: `@OneToOne` (Uni-Directional)](#mapping-onetoone-uni-directional)
    - [Run Database Scripts to Create Tables](#run-database-scripts-to-create-tables)
    - [Create InstructorDetail Class](#create-instructordetail-class)
    - [Create Instructor Class](#create-instructor-class)
    - [Build Main Application](#build-main-application)
    - [Delete Entity](#delete-entity)
  - [Mapping: `@OneToOne` (Bi-Directional)](#mapping-onetoone-bi-directional)
    - [Add New Fields for Instructor Class](#add-new-fields-for-instructor-class)
    - [Develop Main App](#develop-main-app)
    - [Delete Only InstructorDetail](#delete-only-instructordetail)
  - [Mapping: `@OneToMany` (Bi-Directional)](#mapping-onetomany-bi-directional)
    - [Run Database Scripts to Create Tables](#run-database-scripts-to-create-tables-1)
    - [Create Course Class and Define Relationships](#create-course-class-and-define-relationships)
    - [Update Instructor Class](#update-instructor-class)
  - [Mapping: Eager v.s. LazyLoading](#mapping-eager-vs-lazyloading)
    - [Eager Loading](#eager-loading)
    - [Lazy Loading](#lazy-loading)
  - [Mapping: `@OneToMany`  (Non-Directional)](#mapping-onetomany--non-directional)
  - [Mapping: `@ManyToMany`](#mapping-manytomany)

## Overview

### Hibernate Framework

**Hibernate** is a framework for persisting / saving Java objects in a database. It uses JDBC for all database communications.

The benefits of Hibernate are:

- Hibernate handles all of the low-level SQL.
- Minimizes the amount of JDBC code we have to develop.
- Hibernate provides the Object-to-Relational Mapping (ORM).

### Object-To-Relational Mapping (ORM)

**Object-To-Relational Mapping (ORM)** is the relationships that developers define mapping between Java class and database table.

For example, We have Hibernate framework in the middle for mapping the Java class `Student` to the database table `student`.

<div align="center">
  <img src="https://i.imgur.com/y1bPoX7.png" alt="Object-To-Relational Mapping (ORM)">
</div>

We can setup this mapping via configuration **XML file** or **Java Annotations**. And then we can make use of the `session` (the special Hibernate object) to save or retrieve data with database.

```java
// create Java object
Student theStudent = new Student("John", "Doe", "john@luv2code.com");

// save it to database
int theId = (Integer) session.save(theStudent);

// retrieve from database using the primary key
Student myStudent = session.get(Student.class, theId);
```

Moreover, we can querying for Java objects:

```java
Query query = session.createQuery("from Student");
List<Student> students = query.list();
```

### Entity Lifecycle

The Entity Lifecycle is basically a set of states that a Hibernate entity can go through when we're using it in the application:

| Operations  | Description                                                                          |
| :---------: | :----------------------------------------------------------------------------------- |
| **Detach**  | If entity is detached, it's not associated with a Hibernate session.                 |
|  **Merge**  | If instance is detached from session, then merge will reattach to session.           |
| **Persist** | Transitions new instances to managed state. Next flush/commit will save in database. |
| **Remove**  | Transitions managed entity to be removed. Next flush/commit will dete from database. |
| **Refresh** | Reload/synch object with data from database. Prevents stale data.                    |

Let's see the operations and relationship flow:

<div align="center">
  <img src="https://i.imgur.com/F4fZb2f.png" alt="Entity Lifecycle">
</div>

<br/>
<div align="right">
  <b><a href="#hibernate">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Hibernate Annotations

### Development Process

1. Map Class to Database Table.
2. Map Fields to Database Columns.

### Mapping Class and Fields

Let's create a `Student` class and mapping it to the database table:

```java
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "student")
public class Student {
    @Id
    @Column(name = "id")
    private int id;

    @Column(name = "first_name")
    private String firstName;

    @Column(name = "last_name")
    private String lastName;

    @Column(name = "email")
    private String email;

    ...
}
```

JPA is a standard specification and Hibernate is an implementation of the JPA specification. It means that Hibernate implements all of the JPA annotations. And **the Hibernate Team recommends the use of JPA annotations as the best practice instead of Hibernate**.

<br/>
<div align="right">
  <b><a href="#hibernate">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## CRUD: Create, Read, Update and Delete

### Two Key Players

There are two key players in Hibernate:

- **SessionFactory** Class
  - Reads the hibernate config file.
  - Creates Session objects.
  - Heavy-weight object.
  - Only create once in the application.
- **Session** Class
  - Wraps a JDBC connection.
  - Main object used to save/retrieve objects.
  - Short-lived object.
  - Rertrieved from `SessionFactory` Class.

### Create and Save Java Objects

```java
public class CreateStudentDemo {
    public static void main(String[] args) {
        SessionFactory factory = new Configuration()
                                        .configure("hibernate.cfg.xml")
                                        .addAnnotatedClass(Student.class)
                                        .buildSessionFactory();

        // create session
        Session session = factory.getCurrentSession();
        try {
            // create a student object
            System.out.println("Creating new student object");
            Student tempStudent = new Student("Paul", "Wall", "paul@luv2code.com");

            // start transaction
            session.beginTransaction();

            // save the student object
            System.out.println("Saving the student...");
            session.save(tempStudent);

            // commit the transaction
            session.getTransaction().commit();
            System.out.println("Done");

        } finally {
            factory.close();
        }
    }
}
```

### Primary Key

The **Primary Key** is basically a way that we can uniquely identify each row in a table. It should be a unique value and cannot contain `NULL` values. In Hibernate, we use `@Id` annotation to mark a field as the primay key of the class.

```java
@Entity
@Table(name = "student")
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;
    ...
}
```

Furthermore, the `@GeneratedValue()` annotation is used to specifing the generation strategy. Here's the ID generation strategies:

|           Name            | Description                                                                                         |
| :-----------------------: | :-------------------------------------------------------------------------------------------------- |
|   `GenerationType.AUTO`   | pick an appropriate strategy for the particular database.                                           |
| `GenerationType.IDENTITY` | assign primary keys using database identity column. (most commonly used for MySQL `AUTO_INCREMENT`) |
| `GenerationType.SEQUENCE` | assign primary keys using a database sequence.                                                      |
|  `GenerationType.TABLE`   | assign primary keys using an underlying database table to ensure uniqueness.                        |

We can also define our own CUSTOM generation strategy by:

1. Create implementation of `org.hibernate.id.IdentifierGenerator`.
2. Override the method `public Serializable generate()`.


### Retrieve Java Object with Hibernate

```java
// create Java object
Student theStudent = new Student("Daffy", "Duck", "daffy@luv2com.com");

// save it to database
session.save(theStudent);

// now retrieve/read from database using the primary key
Student myStudent = session.get(Student.class, theStudent.getId());
```

### Query Objects

The **Hibernate Query Language (HQL)** is a query language similar to SQL for retrieving objects. So we can use syntax like `where`, `like`, `order by`, `join` and `in`, etc...

```java
// retrieving all students
List<Student> theStudents = session.createQuery("from Student").getResultList();

// retrieving Students: lastName = 'Doe'
List<Student> theStudents = session.createQuery("from Student s where s.lastName='Doe'").getResultList();

// retrieving Students using OR predicate
List<Student> theStudents = session.createQuery("from Student s where s.lastName='Doe' OR s.firstName='Daffy'").getResultList();

// retrieving Students using LIKE predicate
List<Student> theStudents = session.createQuery("from Student s where s.email LIKE '%luv2code.com'").getResultList();
```

It's important that we should **use the Java class name and the Java property name** when using the HQL.

### Update Objects

```java
int studentId = 1;
Student myStudent = session.get(Student.class, studentId);

// update first name to "Scooby"
myStudent.setFirstName("Scooby");

// commit the transaction
session.getTransaction().commit();

// update email for all students
session.createQuery("update Student set email='foo@gmail.com'").executeUpdate();
```

### Delete Objects

```java
int studentId = 1;
Student myStudent = session.get(Student.class, studentId);

// delete the student
session.delete(myStudent);

// commit the transaction
session.getTransaction().commit();

// another way of deleting
session.createQuery("delete from Student where id=2").executeUpdate();
```

<br/>
<div align="right">
  <b><a href="#hibernate">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Advanced Mapping

### Relationships Between Tables

In the database, we most likely will have multiple tables with relationships:

- **One-to-One Mapping**: e.g. An instructor can have an instructor detail entity.
- **One-to-Many Mapping**: e.g. An instructor can have many courses.
- **Many-to-Many Mapping**: e.g. A course can have many students; A student can join many courses.

### Primary Key and Foreign Key

- **Primary Key**: indentify a unique row in a table.
- **Foreign Key**: a field in one table that refers to primary key in another table for linking tables together.

### Cascade

**Cascade** means that we can apply the same operation to related entities.

- If we save an `Instructor`, then it will also cascade that saving operation and apply the same operation to `InstructorDetail`.
- If we delete an `instructor`, we should also delete their `instructor_detail`. (This is known as "CASCADE DELETE")

But the CASCADE DELETE should depend on the use case. There are multiple cascade types provided by JPA and Hibernate:

| Cascade Type | Description                                                                                  |
| :----------: | :------------------------------------------------------------------------------------------- |
| **PERSIST**  | If entity is persisted/saved, related entity will also be persisted.                         |
|  **REMOVE**  | If entity is removed/deleted, related entity will also be deleted.                           |
| **REFRESH**  | If entity is refreshed, related entity will also be refreshed.                               |
|  **DETACH**  | If entity is detached (not associated with session), then related entity will also detached. |
|  **MERGE**   | If entity is merged, then related entity will also be merged.                                |
|   **ALL**    | All of above cascade types.                                                                  |

By default, no operations are cascaded.

### Fetch Types: Eager vs Lazy Loading

There're two types for fetching data from database: Eager Loading and Lazy Loading. It's a best practice to only olad data when absolutely needed, so developers prefer Lazy Loading instead of Eager Loading:

- **Eager Loading**
  - Eager Loading will retrieve everything. e.g. loading instructor and all of their courses at once.
  - Could easily turn into a performance nightmare.
- **Lazy Loading**
  - Lazy Loading will load the main entity first.
  - Load dependent entities on demand (lazy).

We can specify the `fetch` type with `EAGER` or `LAZY` when define the mapping relationship. By the way, different relationship with the different default fetch type:

|    Mapping    | Default Fetch Type |
| :-----------: | :----------------- |
|  `@OneToOne`  | `FetchType.EAGER`  |
| `@OneToMany`  | `FetchType.LAZY`   |
| `@ManyToOne`  | `FetchType.EAGER`  |
| `@ManyToMany` | `FetchType.LAZY`   |

Notice that we require an open Hibernate session to retrieve data. Hibernate will throw an exception when we attempt to retrieve lazy data but the session is closed.

<br/>
<div align="right">
  <b><a href="#hibernate">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Mapping: `@OneToOne` (Uni-Directional)

### Run Database Scripts to Create Tables

Open MySQL Workbench and run following SQL scripts:

```sql
DROP SCHEMA IF EXISTS `hb-01-one-to-one-uni`;

CREATE SCHEMA `hb-01-one-to-one-uni`;

use `hb-01-one-to-one-uni`;

SET FOREIGN_KEY_CHECKS = 0;

DROP TABLE IF EXISTS `instructor_detail`;

CREATE TABLE `instructor_detail` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `youtube_channel` varchar(128) DEFAULT NULL,
  `hobby` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=latin1;


DROP TABLE IF EXISTS `instructor`;

CREATE TABLE `instructor` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `first_name` varchar(45) DEFAULT NULL,
  `last_name` varchar(45) DEFAULT NULL,
  `email` varchar(45) DEFAULT NULL,
  `instructor_detail_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `FK_DETAIL_idx` (`instructor_detail_id`),
  CONSTRAINT `FK_DETAIL` FOREIGN KEY (`instructor_detail_id`) REFERENCES `instructor_detail` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=latin1;

SET FOREIGN_KEY_CHECKS = 1;
```

### Create InstructorDetail Class

```java
// annotate the class as an entity and map to database table
@Entity
@Table(name = "instructor_detail")
public class InstructorDetail {
    // define and annotate fields with database column names
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;

    @Column(name = "youtube_channel")
    private String youtubeChannel;

    @Column(name = "hobby")
    private String hobby;

    // create constructors
    public InstructorDetail() {

    }

    public InstructorDetail(String youtubeChannel, String hobby) {
        this.youtubeChannel = youtubeChannel;
        this.hobby = hobby;
    }

    // generate getter/setter methods
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getYoutubeChannel() {
        return youtubeChannel;
    }

    public void setYoutubeChannel(String youtubeChannel) {
        this.youtubeChannel = youtubeChannel;
    }

    public String getHobby() {
        return hobby;
    }

    public void setHobby(String hobby) {
        this.hobby = hobby;
    }

    // generate toString() method
    @Override
    public String toString() {
        return "InstructorDetail [id=" + id + ", youtubeChannel=" + youtubeChannel + ", hobby=" + hobby + "]";
    }
}
```

### Create Instructor Class

```java
// annotate the class as an entity and map to database table
@Entity
@Table(name = "instructor")
public class Instructor {
    // define and annotate fields with database column names
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;

    @Column(name = "first_name")
    private String firstName;

    @Column(name = "last_name")
    private String lastName;

    @Column(name = "email")
    private String email;

    // set up mapping to InstructorDetail entity
    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "instructor_detail_id")
    private InstructorDetail instructorDetail;

    // create constructors
    public Instructor() {

    }

    public Instructor(String firstName, String lastName, String email) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
    }

    // generate getter/setter methods
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
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

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public InstructorDetail getInstructorDetail() {
        return instructorDetail;
    }

    public void setInstructorDetail(InstructorDetail instructorDetail) {
        this.instructorDetail = instructorDetail;
    }

    // generate toString() method
    @Override
    public String toString() {
        return "Instructor [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + ", email=" + email
                + ", instructorDetail=" + instructorDetail + "]";
    }
}
```

### Build Main Application

```java
public class CreateDemo {
    public static void main(String[] args) {
        SessionFactory factory = new Configuration().configure("hibernate.cfg.xml")
                                                    .addAnnotatedClass(Instructor.class)
                                                    .addAnnotatedClass(InstructorDetail.class)
                                                    .buildSessionFactory();

        // create session
        Session session = factory.getCurrentSession();
        try {
            // create the objects
            Instructor tempInstructor = new Instructor("Chad", "Darby", "darby@luv2code.com");
            InstructorDetail tempInstructorDetail = new InstructorDetail("http://www.luv2code.com/youtube",
                    "Luv 2 code!!!");

            // associate the objects
            tempInstructor.setInstructorDetail(tempInstructorDetail);

            // start transaction
            session.beginTransaction();

            // save the instructor
            // (this will ALSO save the details object because of CascadeType.ALL)
            System.out.println("Saving instructor: " + tempInstructor);
            session.save(tempInstructor);

            // commit the transaction
            session.getTransaction().commit();
            System.out.println("Done");
        } finally {
            factory.close();
        }
    }
}
```

### Delete Entity

```java
public class DeleteDemo {
    public static void main(String[] args) {
        SessionFactory factory = new Configuration().configure("hibernate.cfg.xml")
                                                    .addAnnotatedClass(Instructor.class)
                                                    .addAnnotatedClass(InstructorDetail.class)
                                                    .buildSessionFactory();

        // create session
        Session session = factory.getCurrentSession();
        try {
            // start transaction
            session.beginTransaction();

            // get instructor by primary key /id
            int theId = 1;
            Instructor tempInstructor = session.get(Instructor.class, theId);

            System.out.println("Found instructor: " + tempInstructor);

            // delete the instructors
            // (this will ALSO delete associated "details" object because of CascadeType.ALL)
            if (tempInstructor != null) {
                System.out.println("Deleting: " + tempInstructor);
                session.delete(tempInstructor);
            }

            // commit the transaction
            session.getTransaction().commit();
            System.out.println("Done");
        } finally {
            factory.close();
        }
    }
}
```

<br/>
<div align="right">
  <b><a href="#hibernate">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Mapping: `@OneToOne` (Bi-Directional)

### Add New Fields for Instructor Class

```java
@Entity
@Table(name = "instructor_detail")
public class InstructorDetail {
    ...
    @OneToOne(mappedBy = "instructorDetail", cascade = CascadeType.ALL)
    private Instructor instructor;
    ...
}
```

### Develop Main App

```java
public class GetInstructorDetailDemo {
    public static void main(String[] args) {
        SessionFactory factory = new Configuration().configure("hibernate.cfg.xml")
                                                    .addAnnotatedClass(Instructor.class)
                                                    .addAnnotatedClass(InstructorDetail.class)
                                                    .buildSessionFactory();

        // create session
        Session session = factory.getCurrentSession();
        try {
            // start transaction
            session.beginTransaction();

            // get the instructor detail object
            int theId = 2;
            InstructorDetail tempInstructorDetail = session.get(InstructorDetail.class, theId);

            // print the instructor detail
            System.out.println("tempInstructorDetail: " + tempInstructorDetail);

            // print the associated instructor
            System.out.println("the associated instructor: " + tempInstructorDetail.getInstructor());

            // commit the transaction
            session.getTransaction().commit();
            System.out.println("Done");
        } catch (Exception exc) {
            exc.printStackTrace();
        } finally {
            // handle connection leak issue
            session.close();
            factory.close();
        }
    }
}
```

### Delete Only InstructorDetail

```java
@OneToOne(mappedBy = "instructorDetail",
          cascade = {CascadeType.DETACH, CascadeType.MERGE, CascadeType.PERSIST, CascadeType.REFRESH})
private Instructor instructor;
```

<br/>
<div align="right">
  <b><a href="#hibernate">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Mapping: `@OneToMany` (Bi-Directional)

### Run Database Scripts to Create Tables

Open MySQL Workbench and run following SQL scripts:

```sql
DROP SCHEMA IF EXISTS `hb-03-one-to-many`;

CREATE SCHEMA `hb-03-one-to-many`;

use `hb-03-one-to-many`;

SET FOREIGN_KEY_CHECKS = 0;

DROP TABLE IF EXISTS `instructor_detail`;

CREATE TABLE `instructor_detail` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `youtube_channel` varchar(128) DEFAULT NULL,
  `hobby` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=latin1;


DROP TABLE IF EXISTS `instructor`;

CREATE TABLE `instructor` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `first_name` varchar(45) DEFAULT NULL,
  `last_name` varchar(45) DEFAULT NULL,
  `email` varchar(45) DEFAULT NULL,
  `instructor_detail_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `FK_DETAIL_idx` (`instructor_detail_id`),
  CONSTRAINT `FK_DETAIL` FOREIGN KEY (`instructor_detail_id`)
  REFERENCES `instructor_detail` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=latin1;

DROP TABLE IF EXISTS `course`;

CREATE TABLE `course` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(128) DEFAULT NULL,
  `instructor_id` int(11) DEFAULT NULL,

  PRIMARY KEY (`id`),

  UNIQUE KEY `TITLE_UNIQUE` (`title`),

  KEY `FK_INSTRUCTOR_idx` (`instructor_id`),

  CONSTRAINT `FK_INSTRUCTOR`
  FOREIGN KEY (`instructor_id`)
  REFERENCES `instructor` (`id`)

  ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=latin1;


SET FOREIGN_KEY_CHECKS = 1;
```

### Create Course Class and Define Relationships

```java
@Entity
@Table(name = "course")
public class Course {
    // define our fields
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;

    @Column(name = "title")
    private String title;

    @ManyToOne(cascade = { CascadeType.PERSIST, CascadeType.MERGE, CascadeType.DETACH, CascadeType.REFRESH })
    @JoinColumn(name = "instructor_id")
    private Instructor instructor;

    // define constructors
    public Course() {

    }

    public Course(String title) {
        this.title = title;
    }

    // define getter and setters
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public Instructor getInstructor() {
        return instructor;
    }

    public void setInstructor(Instructor instructor) {
        this.instructor = instructor;
    }

    // define toString
    @Override
    public String toString() {
        return "Course [id=" + id + ", title=" + title + "]";
    }
}
```

### Update Instructor Class

```java
@Entity
@Table(name = "instructor")
public class Instructor {
    ...
    @OneToMany(mappedBy = "instructor",
               cascade = { CascadeType.PERSIST, CascadeType.MERGE, CascadeType.DETACH, CascadeType.REFRESH })
    private List<Course> courses;
    ...
    // add convenience methods for bi-directional relationship
    public void add(Course tempCourse) {
        if (courses == null) {
            courses = new ArrayList<>();
        }

        courses.add(tempCourse);
        tempCourse.setInstructor(this);
    }
}
```

<br/>
<div align="right">
  <b><a href="#hibernate">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Mapping: Eager v.s. LazyLoading

### Eager Loading

```java
@Entity
@Table(name = "instructor")
public class Instructor {
    ...
    @OneToMany(fetch = FetchType.EAGER,
               mappedBy = "instructor",
               cascade = { CascadeType.PERSIST, CascadeType.MERGE, CascadeType.DETACH, CascadeType.REFRESH })
    private List<Course> courses;
}
```

### Lazy Loading

```java
@Entity
@Table(name = "instructor")
public class Instructor {
    ...
    @OneToMany(fetch = FetchType.LAZY,
               mappedBy = "instructor",
               cascade = { CascadeType.PERSIST, CascadeType.MERGE, CascadeType.DETACH, CascadeType.REFRESH })
    private List<Course> courses;
}
```

<br/>
<div align="right">
  <b><a href="#hibernate">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Mapping: `@OneToMany`  (Non-Directional)

<br/>
<div align="right">
  <b><a href="#hibernate">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Mapping: `@ManyToMany`

<br/>
<div align="right">
  <b><a href="#hibernate">[ ↥ Back To Top ]</a></b>
</div>
<br/>