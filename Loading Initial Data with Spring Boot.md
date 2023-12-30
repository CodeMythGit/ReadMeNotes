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
