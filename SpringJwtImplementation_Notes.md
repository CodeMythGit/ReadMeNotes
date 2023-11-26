# JWT Authentication using Spring Boot Notes

## Step To Followed 

        1. Create Basic Rest API
        2. Secure the Rest API by adding security dependecy
        3. Use the properties file to create custom username and password for authentication
        4. Create the SpringSecurityConfig class to define the bean like PasswordEncoder, UserDetailsService, AuthenticationManager and SecurityFilterChain
        5. Create JwtAuthenticationEntryPoint Class Which implements AuthenticationEntryPoint interface and override method commence

## Important Dependency to be used
1. For rest api

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

2. For Getter and Setter
   
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

3. For Security

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

4. For JWT

        <!-- https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt-api -->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-api</artifactId>
            <version>0.11.5</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/io.jsonwebtoken/jjwt-impl -->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-impl</artifactId>
            <version>0.11.5</version>
            <scope>runtime</scope>
        </dependency>

        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-jackson</artifactId> <!-- or jjwt-gson if Gson is preferred -->
            <version>0.11.5</version>
            <scope>runtime</scope>
        </dependency>


## Create Class JwtAuthenticationEntryPoint and implements AuthenticationEntryPoint

Method of this class is called whenever as exception is thrown due to unauthenticated user trying to access the resource that required authentication.

```java
@Component
public class JwtAuthenticationEntryPoint implements AuthenticationEntryPoint {
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
        PrintWriter writer = response.getWriter();
        writer.println("Access Denied !! Message :" + authException.getMessage());
    }
}
```
    


    
