# How To Implement Spring Boot Retryable Features

## Step  to implement

* Important Dependencies to be used
* Important Annotation to implement Retryable
* Implementation of code in IDE

## Step 1 : Important Dependencies to be used

        <dependency>
            <groupId>org.springframework.retry</groupId>
            <artifactId>spring-retry</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

## Step 2 :Important Annotation to implement Retryable

* **@EnableRetry**

  * The **@EneableRetry** annotation enables the spring retry feature in the application. We can apply it to any **@Confguration** class.
  * The **@EnableRetry** scan for all **@Retryable** and **@Recover** annotated methods and proxies them using AOP.
  * It also configures RetryListener interfaces used for intercepting methods used during retries.

 ```java
@EnableRetry
@Configuration
public class RetryConfig {

  
}
```

* **@Retryable**

    * It indicates a method to be a candidate for retry. We specify the **exception type for which the retry should be done**, the maximum number of retries and the delay between two retries using the **backoff policy**.

> [!NOTE]
> Please note that **maxAttampts** includes the first failure also. In other words, the first invocation of this method is always the first retry attempt. The **default retry count is 3**.

```java
public interface BackendAdapter {

    @Retryable(retryFor = {RemoteServiceNotAvailableException.class},
            maxAttempts = 3,
            backoff = @Backoff(delay = 1000))
    public String getBackendResponse(String param1, String param2);

    ...
}
```

* **@Recover**

    * It specifies the **fallback method if all retries fail** for a **@Retryable** method. The **method accepts the exception that occurred** in the Retryable method and its **arguments in the same order**.



```java
public interface BackendAdapter {

    ...

    @Recover
    public String getBackendResponseFallback(RemoteServiceNotAvailableException e, String param1, String param2);

}
```
