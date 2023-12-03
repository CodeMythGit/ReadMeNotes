# File Upload Download Using Spring Boot Rest API Thymeleaf

## Preview of File Upload Application
![Screenshot 2023-12-03 222010](https://github.com/CodeMythGit/ReadMeNotes/assets/90126232/31fc4096-6bd8-405d-bb77-c1a778ceb6a1)


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
## Create index.html
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head th:replace="~{base :: head(~{::title})}">
  <title>File Upload Download Application</title>
</head>
<body>
<div class="container">
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container">
      <a class="navbar-brand" href="#">CodeMyth</a>
    </div>
  </nav>
<h1>File Upload Download Application</h1>
<section>
  <div class="items">
    <form class="form-horizontal" role="form" th:action="@{/update}" th:object="${items}" method="POST">
      <table class="table table-bordered table-striped" id="todoItems">
        <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>FileType</th>
          <th>Action</th>
        </tr>
        </thead>
        <tbody>
        <tr th:each="item,i : *{itemList}">
          <input type="hidden" th:field="*{itemList[__${i.index}__].id}" />
          <td th:text="${item.id}"></td>
          <td th:text="${item.fileName}"></td>
          <td th:text="${item.fileType}"></td>
          <td>
            <a th:href="@{/download(id=${item.id})}"
               title="Delete this item" class="fa-solid fa-download"></a>
            <a th:href="@{/delete(id=${item.id})}"
               title="Delete this item" class="fa-regular fa-trash-can icon-dark btn-delete"></a>
          </td>
        </tr>
        </tbody>
      </table>
    </form>
  </div>
  <hr />

  <!-- Item Input Form -->
  <div class="input-group">
    <form class="form-horizontal" role="form" th:action="@{/upload}" th:object="${newitem}" method="POST"
          enctype="multipart/form-data">
      <div class="custom-file">
        <input type="file" class="custom-file-input" id="inputGroupFile01" name="files">
      </div>
      <br>
      <button type="submit" class="btn btn-primary">Upload</button>
    </form>
  </div>
</section>
</div>
</body>
</html>
```

## Create base.html
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head th:fragment="head(title)">
    <title th:replace="${title}">File Upload Download Application</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <!-- Bootstrap -->
    <!-- Latest compiled and minified CSS -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.3.2/css/bootstrap.min.css" integrity="sha512-b2QcS5SsA8tZodcDtGRELiGv5SaKSk1vDHDaQRda0htPYWZ6046lr3kJ5bAAQdpV2mmA/4v0wQF9MyU6/pDIAg==" crossorigin="anonymous" referrerpolicy="no-referrer" />
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.3.2/js/bootstrap.min.js" integrity="sha512-WW8/jxkELe2CAiE4LvQfwm1rajOS8PHasCCx+knHG0gBHt8EXxS6T6tJRTGuDQVnluuAvMxWF4j8SNFDKceLFg==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css" integrity="sha512-DTOQO9RWCH3ppGqcWaEA1BIZOC6xxalwEsw9c2QQeAIftl+Vegovlnee1c9QX4TctnWMn13TZye+giMm8e2LwA==" crossorigin="anonymous" referrerpolicy="no-referrer" />
</head>
<body style="padding-top:50px;" th:fragment="body(heading, content)">
<!-- Nav Bar -->
<div class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container">
        <div class="navbar-header">
            <a class="navbar-brand" href="#">File Upload Download Application</a>
        </div>
    </div>
</div>
<!-- Body -->
<div class="container">
    <h1 th:replace="${heading}">Page Heading</h1>
    <hr />
    <th:block th:replace="${content}" />
</div>
</body>
</html>
```
## Create FileController

```java
@Controller
@AllArgsConstructor
public class FileController {

    private FileItemRepository repository;

    private final String FOLDER_PATH = "D:/uploads/";

    @PostMapping("/upload")
    public String uploadFile(@RequestParam("files") MultipartFile file, Model model) throws IOException {
        String filePath = FOLDER_PATH + file.getOriginalFilename();
        FileItem item = FileItem.builder()
                .fileName(file.getOriginalFilename())
                .fileType(file.getContentType())
                .location(filePath)
                .build();
        file.transferTo(new File(filePath));
        repository.save(item);
        return "redirect:/";
    }

    @GetMapping("/")
    public String success(Model model) {
        List<FileItem> items = repository.findAll();
        model.addAttribute("items", new FileItemViewModel(items));
        model.addAttribute("newitem", new FileItem());
        return "index";
    }

    @GetMapping("/download")
    public ResponseEntity<Object> downloadFile(@RequestParam("id") Integer id) throws IOException {
        Optional<FileItem> fileItem = repository.findById(id);
        String path = fileItem.get().getLocation();
        byte[] bytes = Files.readAllBytes(new File(path).toPath());
        return ResponseEntity.ok()
                .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + fileItem.get().getFileName() + "\"").body(bytes);

    }

    @GetMapping("/delete")
    public String deleteFile(@RequestParam("id") Integer id, Model model) throws IOException {
        repository.deleteById(id);
        List<FileItem> items = repository.findAll();
        model.addAttribute("items", new FileItemViewModel(items));
        model.addAttribute("newitem", new FileItem());
        return "index";
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
@Table(name = "file_items")
@Builder
public class FileItem {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private Integer id;
    private String fileName;
    private String fileType;
    private String location;
}
```

## Create view model
```java
@NoArgsConstructor
@AllArgsConstructor
@Getter
@Setter
public class FileItemViewModel {

    private List<FileItem> itemList = new ArrayList<>();

    public FileItemViewModel(Iterable<FileItem> list) {
        list.forEach(itemList::add);
    }
}
```

## Creating Repository
```java
@Repository
public interface FileDataRepository extends JpaRepository<FileData, Integer> {
    Optional<FileData> findByName(String filename);
}
```
