# Learn Exception Handling using Realtime project


## Design Overview

![image](https://github.com/CodeMythGit/ReadMeNotes/assets/90126232/5e6a4f4d-e27b-406a-a8d8-4b9b9d7ff8e9)

## Important method to be implemented.

![image](https://github.com/CodeMythGit/ReadMeNotes/assets/90126232/3d8edebf-e0e7-496f-80be-d90c4a7cc552)

## Project Structure

![image](https://github.com/CodeMythGit/ReadMeNotes/assets/90126232/26209926-d4c6-40f0-a968-1c6c99c519c0)



## Source Code
1. index.html


```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">

<head>
  <title>Student Form</title>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />

  <!-- Bootstrap -->
  <!-- Latest compiled and minified CSS -->
  <link rel="stylesheet"
        href="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css"
        integrity="sha384-HSMxcRTRxnN+Bdg0JdbxYKrThecOKuH5zCYotlSAcp1+c8xmyTe9GYg1l9a69psu"
        crossorigin="anonymous">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.2.0/css/all.min.css"
        integrity="sha512-xh6O/CkQoPOWDdYTDqeRdPCVd1SpvCA9XXcUnZS2FmJNp1coAFzvtCN9BmamE+4aHK8yyUHUSCcJHgXloTyT2A=="
        crossorigin="anonymous" referrerpolicy="no-referrer" />
  <script src="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"
          integrity="sha384-aJ21OjlMXNL5UyIl/XNwTMqvzeRMZH2w8c5cRVpzpU8Y5bApTppSuUkhZXN0VxHd"
          crossorigin="anonymous">
  </script>
</head>

<body th:class="container">
  <h2 style="color: white;background-color: black">CodeMyth - Student Validation Form</h2>

  <section>
    <div class="studentList">
      <th th:if="${message != ''}">
        <div class="alert alert-success" role="alert">
          <span th:text="${message}"></span>
        </div>
      </th>
      <h3>Student List</h3>
      <form class="form-horizontal" role="form" th:action="@{/update}" th:object="${studentList}" method="POST">
        <table class="table table-bordered table-striped" id="todoItems">
          <thead>
            <tr>
              <th>Name</th>
              <th>City</th>
              <th>Age</th>
              <th>Email</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr th:each="stud,i : *{studentList}">
              <input type="hidden" th:field="*{studentList[__${i.index}__].id}" />
              <td th:text="${stud.name}"></td>
              <td th:text="${stud.city}">
              <td th:text="${stud.age}"></td>
              <td th:text="${stud.email}"></td>
              <td>
                <a th:href="@{/delete(id=${stud.id})}"
                   title="Delete Student" class="fa-regular fa-trash-can icon-dark btn-delete"></a>
                <a th:href="@{/edit(id=${stud.id})}"
                   title="Delete Student" class="fa-regular fa-edit icon-dark"></a>
              </td>
            </tr>
          </tbody>
        </table>
      </form>
    </div>

    <hr />

    <!-- Item Input Form -->
    <div class="studentForm">
      <h3>Student Form</h3>
      <form class="form-horizontal" role="form" th:action="@{/save}" th:object="${student}" method="POST">
        <br><br>
        <input type="hidden" th:field="${student.id}" name="id"/>
        <div class="form-group" th:class="all-classes-container">
          <label for="name" class="col-sm-2">Name</label>
          <div class="col-sm-10">
            <input type="text" th:field="*{name}" class="form-control" id="name" placeholder="Enter Student Name" />
            <p th:if="${#fields.hasErrors('name')}" class="label label-danger" th:errors="*{name}"></p>
          </div>
        </div>

        <div class="form-group" th:class="all-classes-container">
          <label for="city" class="col-sm-2">City</label>
          <div class="col-sm-10">
            <input type="text" th:field="*{city}" class="form-control" id="city" placeholder="Enter Student City" />
            <p th:if="${#fields.hasErrors('city')}" class="label label-danger" th:errors="*{city}"></p>
          </div>
        </div>

        <div class="form-group" th:class="all-classes-container">
          <label for="email" class="col-sm-2">Email</label>
          <div class="col-sm-10">
            <input type="email" th:field="*{email}" class="form-control" id="email" placeholder="Enter Student Email" />
            <p th:if="${#fields.hasErrors('email')}" class="label label-danger" th:errors="*{email}"></p>
          </div>
        </div>

        <div class="form-group" th:class="all-classes-container">
          <label for="age" class="col-sm-2">Age</label>
          <div class="col-sm-10">
            <input type="number" th:field="*{age}" class="form-control" id="age" placeholder="Enter Student Age" />
            <p th:if="${#fields.hasErrors('age')}" class="label label-danger" th:errors="*{age}"></p>
          </div>
        </div>

        <button type="submit" class="btn btn-primary">Add Student</button>

      </form>
    </div>
  </section>


</body>

</html>
```

2. Controller Class

```java
package com.codemyth.controller;

import com.codemyth.model.Student;
import com.codemyth.model.StudentViewModel;
import com.codemyth.repository.StudentRepository;
import jakarta.validation.Valid;
import lombok.AllArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import java.util.List;
import java.util.Objects;

@Controller
@Slf4j
@AllArgsConstructor
public class StudentController {

    private StudentRepository repository;

    @GetMapping({"/", "/edit"})
    public String homePage(Model model, @ModelAttribute("message") String message, @RequestParam(value = "id",required = false) Integer id) {
        List<Student> studentList = repository.findAll();
        if(!Objects.isNull(id)){
            model.addAttribute("student", repository.findById(id).get());
        }else{
            model.addAttribute("student", Student.builder().build());
        }
        model.addAttribute("studentList", StudentViewModel.builder().studentList(studentList).build());
        model.addAttribute("message", message);
        return "index";
    }

    @PostMapping("/save")
    public String saveStudent(@Valid @ModelAttribute("student") Student student,BindingResult result,
                              RedirectAttributes redirectAttributes,
                              Model model) {
        if(result.hasFieldErrors()) {
            List<Student> studentList = repository.findAll();
            model.addAttribute("student", student);
            model.addAttribute("studentList", StudentViewModel.builder().studentList(studentList).build());
            model.addAttribute("message", "Please correct the student form");
            return "index";
        }

        if(student.getId() == null){
            redirectAttributes.addFlashAttribute("message", "Student saved successfully...");
        }else{
            redirectAttributes.addFlashAttribute("message", "Student Updated successfully...");
        }

        log.info("Student details " + student.toString());
        repository.save(student);
        return "redirect:/";
    }

    @GetMapping("/delete")
    public String deleteStudent(@RequestParam("id") Integer id, RedirectAttributes attributes) {
        attributes.addFlashAttribute("message", "Student Deleted successfully for " + id);
        repository.deleteById(id);
        return "redirect:/";
    }

}

```

