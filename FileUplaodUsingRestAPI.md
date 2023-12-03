# File Upload Download Using Spring Boot Rest API

# Requirement

> [!NOTE]
> 1. Any Favourite IDE.
> 2. Postman for testing endpoint
> 3. For Database we will use **PostgreSql**
> 4. Good interet connection to build project faster

# Important Dependency

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <scope>runtime</scope>
        </dependency>
        
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

# Flow Diagram 

![Screenshot 2023-12-03 220452](https://github.com/CodeMythGit/ReadMeNotes/assets/90126232/5be13fff-63d8-4a5d-9ea4-97d15be51d2e)


# Code
## Create FileUploadDownloadController

```java
@RestController
@RequestMapping("/file")
@AllArgsConstructor
public class FileUploadDownloadController {

    private FileDataRepository repository;

    private final String FOLDER_PATH = "D:/uploads/";

    @PostMapping("/upload")
    public ResponseEntity<Object> uploadFile(@RequestParam("file") MultipartFile file) throws IOException {
        String filePath = FOLDER_PATH + file.getOriginalFilename();
        FileData fileData = FileData.builder()
                .name(file.getOriginalFilename())
                .type(file.getContentType())
                .path(filePath)
                .build();
        repository.save(fileData);

        file.transferTo(new File(filePath));
        return ResponseEntity.status(HttpStatus.CREATED).body("File uploaded successfully to " + filePath);
    }

    @GetMapping("/download/{filename}")
    public ResponseEntity<Object> downloadFile(@PathVariable String filename) throws IOException {
        Optional<FileData> fileData = repository.findByName(filename);
        String filePath = fileData.get().getPath();
        byte[] file = Files.readAllBytes(new File(filePath).toPath());
        return ResponseEntity.status(HttpStatus.OK)
                .contentType(MediaType.valueOf("image/png"))
                .body(file);
    }
}

```

## Creating Model 
```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@Entity
@Table(name = "file_data")
@Builder
public class FileData {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;
    private String name;
    private String type;
    private String path;
}
```

## Creating Repository
```java
@Repository
public interface FileDataRepository extends JpaRepository<FileData, Integer> {
    Optional<FileData> findByName(String filename);
}
```
