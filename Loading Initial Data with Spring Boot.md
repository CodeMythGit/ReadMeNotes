# Topic : Loading Initial Data with Spring Boot

> [!TIP]
> We are going to learn the theoretical and practical implementation of concepts for better understanding. 🔔

## Topic Covered in this video
>1. Overview
>2. The data.sql File
>3. The schema.sql File
>4. Controlling Database Creation Using Hibernate
>5. Customizing Database Schema Creation

## Point 1 : Overview (What is use of this)

Spring Boot simplifies database management by automatically creating tables for entities in packages, but allows for fine-grained control through data.sql and schema.sql files.

## Point 2 : The data.sql File

Assuming we are working with JPA and defining a simple Country entity in our project.

```java
@Entity
public class Country {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Integer id;

    private String name;
}
```

Spring Boot **creates an empty table without populating** it with anything, which can be done by creating a file named **data.sql**.
```sql
INSERT INTO country (name) VALUES ('India');
INSERT INTO country (name) VALUES ('Brazil');
INSERT INTO country (name) VALUES ('USA');
INSERT INTO country (name) VALUES ('Italy');
```
To defer the initialization of our data source, we can use the property provided to execute data.sql scripts before Hibernate is initialized.

```properties
spring.jpa.defer-datasource-initialization=true
```

The project will populate the country table when run with the specified file on the classpath. For **script-based initialization**, such as **inserting data or creating a schema**, the following property must be set:
```properties
spring.sql.init.mode=always
```

## Point 3 : The schema.sql File

Sometimes, we don’t want to rely on the default schema creation mechanism.
In such cases, we can create a custom **schema.sql** file:

```sql
CREATE TABLE IF NOT EXISTS country (
    id SERIAL PRIMARY KEY,
    NAME varchar(100) not null
);
```
> [!IMPORTANT]
> Using script-based initialization through schema.sql and data.sql, along with Hibernate initialization, can cause issues when combined.

To solve this, we can **disable the execution of DDL commands** altogether by Hibernate, which Hibernate uses for the **creation/updation** of tables:
```properties
spring.jpa.hibernate.ddl-auto=none
```
This will ensure that **only script-based schema** generation is performed using **schema.sql**.

To achieve Hibernate automatic schema generation in conjugation with script-based schema creation and data population, we will need to use:
```properties
spring.jpa.defer-datasource-initialization=true
```

After creating a Hibernate schema, it is crucial to read schema.sql for any additional schema changes and execute data.sql to populate the database.
To always initialize a database using scripts, we’ll have to use:
```properties
spring.sql.init.mode=always
```

## Point 4 : Controlling Database Creation Using Hibernate
The standard Hibernate property values are create, update, create-drop, validate and none:

* **create** – Hibernate first drops existing tables and then creates new tables.
* **update** – The object model created based on the mappings (annotations or XML) is compared with the existing schema, and then Hibernate updates the schema according to the diff. It never deletes the existing tables or columns even if they are no longer required by the application.
* **create-drop** – similar to create, with the addition that Hibernate will drop the database after all operations are completed; **typically used for unit testing**
* **validate** – Hibernate only validates whether the tables and columns exist; otherwise, it throws an exception.
* **none** – This value effectively turns off the DDL generation.

## Point 5 : Customizing Database Schema Creation

**By default, Spring Boot automatically creates the schema of an embedded DataSource.

The spring.sql.init.mode property can be used to control or customize this behavior, taking one of three values.

* **always** – always initialize the database
* **embedded** – always initialize if an embedded database is in use. This is the default if the property value is not specified.
* **never** – never initialize the database

> [!NOTE]
> This property was introduced in **Spring Boot 2.5.0**; we need to use **spring.datasource.initialization-mode** if we are using previous versions of Spring Boot.
