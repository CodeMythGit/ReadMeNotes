# 8 WAYS TO IMPROVE YOUR SPRING BOOT SKILLS

Below are the points that we are going to discuss:
1. Good Understanding of Starter POM(Project Object Model
2. Externalize Your Configuration
3. You Need To Keep Your Controllers Lean
4. Handle Exception Globally
5. Make Use Of Lazy Initialization
6. Take Observability Seriously
7. Master Your Build Tool Of Preference



## [1. Good Understanding of Starter POM(Project Object Model)](url)

  Starter POMs removes the pain of looking for and configuring common dependencies in your project.

  The spring-boot-starter-data-jpa is a starter for using Spring Data JPA. It includes the following compiled dependencies:

* **spring-boot-starter-jdbc:** This starter includes the necessary libraries for **JDBC**.
* **hibernate-core:** This is the main library for Hibernate, which is an **Object-Relational Mapping (ORM)** solution.
* **spring-data-jpa:** This library provides the core **Spring Data JPA functionality**.
* **spring-boot-starter-aop:** This library enables **Aspect-Oriented Programming** in Spring Boot which addresses cross-cutting concerns for cleaner, modular code.
* **spring-aspects:** This library is typically **used when you need the advanced features** provided by AspectJ, such as weaving into third-party libraries.

## [2. Externalize Your Configuration](url)

* Another way to utilize Spring Boot to its full potential is to **try as much as possible to externalize your configuration** instead of hard coding them. Externalizing your configuration will make your application more flexible and easier to manage.
* Another advantage of externalizing your configuration is that you **don’t need to shut down your application**, make a change, recompile, and then redeploy your application **just because of a small change**.
* To externalize your configuration, you need to make use of the **application.properties** or **application.yml**

Lets Understand with Example in IDE

## [3. You Need To Keep Your Controllers Lean](url)

 Another way to utilize Spring Boot to its full potential is to ensure that you keep your controllers lean. Keeping your controllers lean will help you avoid practices that will lead to the ‘Fat Controller’ anti-pattern.
### What is the ‘Fat Controller’ anti-pattern?

The **‘Fat Controller’** anti-pattern is a pattern in programming where **controllers in a web application become overloaded with business logic**, violating the principles of separation of concerns. When controllers take on too much responsibility, the following issues are bound to arise:

* **Maintainability**: Large controllers are difficult to maintain and understand. Any modification or addition to business logic becomes error-prone and complex.
* **Testability**: With business logic being embedded in controllers, it becomes difficult to carry out tests in isolation, making it challenging to create reliable unit tests.
* **Reusability**: You hardly can find reusable logic in controllers. If the same logic is needed in another part of the application, it is usually very difficult to extract and reuse.
* **Readability**: A codebase becomes less readable when you have business logic in controllers. It becomes difficult for developers to quickly understand the purpose and functionality of a controller.

To avoid the **‘Fat Controller’** anti-pattern, adhere to the following principles:

* **Stateless**: By default controllers are singletons and any state change can cause a lot of issues.
* **Delegating**: Your controllers should be delegating, meaning it should not execute business logic, but rely on delegation.
* **Encapsulated**: The HTTP layer of your application should be handled by controllers, this should not be handled by the service layer.
* **Use-case-centric**: make sure your controllers are built around use cases or the business domain.

## [4. Handle Exception Globally](url)

* To handle exceptions globally, you need to create a class and annotate this class with this annotation **@ControllerAdvice (Web MVC projects)** or **@RestControllerAdvice (Web API projects)**.
* **@ControllerAdvice** is designed for Spring Boot MVC applications, it is an annotation that when applied to a class, transforms it into a global controller advisor, in turn, influences the behavior of multiple controllers.
* Lets Understand with Example in IDE


## [5. Make Use Of Lazy Initialization](url)

* The SpringApplication allows you as a developer to initialize your spring boot application lazily. **When lazy initialization is enabled, beans get created as they are needed** rather than during application startup. Enabling lazy initialization reduces the time that it takes your application to start.
* Lazy initialization results in many web-related beans not being initialized until an HTTP request is received.
* The downside of lazy initialization is that it delays the discovery of a problem with the application. If you lazily initialize a misconfigured bean, the failure of that bean will not occur during startup and the problem will only show up when the bean is initialized.

You need to ensure that the JVM has sufficient memory to accommodate all of the application’s beans and not just those that are initialized during startup. You need to fine-tune the JVM’s heap size before you enable lazy initialization.

Lazy initialization can be enabled programmatically using:

>1. **lazyInitialization** method on SpringApplicationBuilder
>2. **setLazyInitialization** method on SpringApplication
>3. **spring.main.lazy-initialization** property

## [6. Take Observability Seriously](url)

* Observability is basically the **process of measuring the internal state of your application** using its external outputs such as logs, metrics and traces.

Actuator Endpoints dependency :

  ![image](https://github.com/CodeMythGit/ReadMeNotes/assets/90126232/5df01154-e3a2-42ca-8d7e-2f45c8e47f1c)

* One thing you need to know is that, Actuator comes with most of its endpoints disabled, **leaving just two endpoints enabled /health and /info**
* **To enable all endpoints**, go to your **application.properties** or **application.yml** file and add the following
  
![image](https://github.com/CodeMythGit/ReadMeNotes/assets/90126232/80951e03-3abe-4775-a048-3fa0ff2e842b)

### Some interesting Actuator Endpoints
  * **/health**: This endpoint shows you the **basic health information of your application**. It is one of the most commonly used endpoints and is exposed by default.
  * **/info**: This endpoint will **show you the application information**. This is useful when you want to expose details like the application name, version, and any custom properties.
  * **/beans**: This endpoint shows you a comprehensive **list of all the Spring beans in your application**.
  * **/loggers**: This endpoint shows you the **configuration of loggers** in the application.
  * **/env**: This endpoint shows the **properties defined in the environment of your application**. These properties can include system properties, environment variables, and properties from application.properties or application.yml.
  * **/mappings**: This endpoint shows a **list of all @RequestMapping paths** in your application.
  * **/configprops**: This endpoint displays a **list of collated @ConfigurationProperties**

## 7. Master Your Build Tool Of Preference

  One way to use Spring Boot to its fullest is to master the build tool of your preference like **maven** or **gradle**.
