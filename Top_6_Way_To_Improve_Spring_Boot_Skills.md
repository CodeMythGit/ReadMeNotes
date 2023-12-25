# 8 WAYS TO IMPROVE YOUR SPRING BOOT SKILLS

## [1. Good Understanding of Starter POM(Project Object Model)](url)

  Starter POMs removes the pain of looking for and configuring common dependencies in your project.

  The spring-boot-starter-data-jpa is a starter for using Spring Data JPA. It includes the following compiled dependencies:

>1.  **spring-boot-starter-jdbc:** This starter includes the necessary libraries for JDBC.
>2.  **hibernate-core:** This is the main library for Hibernate, which is an Object-Relational Mapping (ORM) solution.
>3.  **spring-data-jpa:** This library provides the core Spring Data JPA functionality.
>4.  **spring-boot-starter-aop:** This library enables Aspect-Oriented Programming in Spring Boot which addresses cross-cutting concerns for cleaner, modular code.
>5.  **spring-aspects:** This library is typically used when you need the advanced features provided by AspectJ, such as weaving into third-party libraries.

## [2. Externalize Your Configuration](url)

* Another way to utilize Spring Boot to its full potential is to try as much as possible to externalize your configuration instead of hard coding them. Externalizing your configuration will make your application more flexible and easier to manage.
* Another advantage of externalizing your configuration is that you don’t need to shut down your application, make a change, recompile, and then redeploy your application just because of a small change.
* Here is how you can externalize your configuration
* To externalize your configuration, you need to make use of the application.properties or application.yml

Lets Understand with Example in IDE

## [3. You Need To Keep Your Controllers Lean](url)

* Another way to utilize Spring Boot to its full potential is to ensure that you keep your controllers lean. Keeping your controllers lean will help you avoid practices that will lead to the ‘Fat Controller’ anti-pattern.
### What is the ‘Fat Controller’ anti-pattern?

The ‘Fat Controller’ anti-pattern is a pattern in programming where controllers in a web application become overloaded with business logic, violating the principles of separation of concerns. When controllers take on too much responsibility, the following issues are bound to arise:

* **Maintainability**: Large controllers are difficult to maintain and understand. Any modification or addition to business logic becomes error-prone and complex.
* **Testability**: With business logic being embedded in controllers, it becomes difficult to carry out tests in isolation, making it challenging to create reliable unit tests.
* **Reusability**: You hardly can find reusable logic in controllers. If the same logic is needed in another part of the application, it is usually very difficult to extract and reuse.
* **Readability**: A codebase becomes less readable when you have business logic in controllers. It becomes difficult for developers to quickly understand the purpose and functionality of a controller.

To avoid the ‘Fat Controller’ anti-pattern, adhere to the following principles:

* **Stateless**: By default controllers are singletons and any state change can cause a lot of issues.
* **Delegating**: Your controllers should be delegating, meaning it should not execute business logic, but rely on delegation.
* **Encapsulated**: The HTTP layer of your application should be handled by controllers, this should not be handled by the service layer.
* **Use-case-centric**: make sure your controllers are built around use cases or the business domain.
