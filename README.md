# 🌟 Complete Guide to JPA Relationships (Beginner Friendly)

This article explains **everything** about **JPA relationships**:
- One-To-One 🔗
- One-To-Many 📚
- Many-To-One 🔁
- Many-To-Many 🔀
- Owning side vs Inverse side 🎯
- Lazy vs Eager loading ⚡
- How JSON gets created 📦
- `mappedBy` meaning 🛠️

> 📖 After reading this, you won't need to search anything else!

---

# 📚 Basics You Must Know First

- **Entity**: A Java class mapped to a database table.
- **Primary Key**: Unique identifier (`@Id`) for a record.
- **Foreign Key**: A field linking one table to another.

---

<details>
<summary>1️⃣ One-To-One Relationship 🔗</summary>

### ➡️ Meaning:
One record in Table A links to exactly one record in Table B.

---

### 🛢 Table Structure

| User Table |   | Profile Table |
|:-----------|---|:--------------|
| id (PK)    |   | id (PK)        |
| name       |   | bio            |
| profile_id (FK) → Profile(id) | | |

---

### 🔥 Java Code

```java
@Entity
public class User {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToOne
    @JoinColumn(name = "profile_id")
    private Profile profile;
}

@Entity
public class Profile {
    @Id
    @GeneratedValue
    private Long id;

    private String bio;
}
```
### 🧠 Explanation
- @OneToOne: Each User has one Profile.

- @JoinColumn(name = "profile_id"): User table will have a profile_id foreign key.

Profile table doesn't have any user reference unless you want a bi-directional relationship.

</details>
<details> 
<summary>2️⃣ One-To-Many and Many-To-One Relationship 📚🔁</summary>

### ➡️ Meaning:
-- One record in Table A links to many records in Table B.

-- Each record in Table B belongs to one record in Table A.

---

### 🛢 Table Structure

| Department Table |   | Employee Table |
|:-----------|---|:--------------|
| id (PK)    |   | id (PK)        |
| name       |   | name            |
| department_id (FK) → Department(id) | | |

---

### 🔥 Java Code

```java
@Entity
public class Department {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "department")
    private List<Employee> employees;
}

@Entity
public class Employee {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;
}
```

🧠 Explanation
- @OneToMany(mappedBy = "department"): Department is inverse side. Employee is owning side.

- mappedBy = "department": Refers to the department field in Employee.

- @ManyToOne: Each employee belongs to one department.

- @JoinColumn(name = "department_id"): Creates the foreign key in Employee table.

</details>

<details> 
<summary>3️⃣ Many-To-Many Relationship 🔀</summary>

### ➡️ Meaning:
-- Many records in Table A link to many records in Table B.

-- An extra Join Table is needed.

---

## 🛢 Table Structure

| Student Table | Course Table | student_course (Join Table) |
|---------------|--------------|-----------------------------|
| id (PK)       | id (PK)       | student_id (FK)             |
| name          | title         | course_id (FK)              |
---

### 🔥 Java Code

```java
@Entity
public class Student {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private List<Course> courses;
}


@Entity
public class Course {
    @Id
    @GeneratedValue
    private Long id;

    private String title;

    @ManyToMany(mappedBy = "courses")
    private List<Student> students;
}
```
🧠 Explanation
- @ManyToMany: Links many students to many courses.

- @JoinTable: Defines the intermediate table student_course.

- joinColumns: References the Student.

- inverseJoinColumns: References the Course.

- mappedBy = "courses": Course side is inverse, Student side owns the relationship.

</details>

<details> 
  <summary>🎯 Owning Side vs Inverse Side 🔄</summary>

  🧠 Key Points
   - Owning Side:

     -- Has the @JoinColumn.

     -- Controls the database updates.

   - Inverse Side:

     -- Has the mappedBy.

     -- Is just a mirror.


---

### 🔥 Java Code

```java
// Owning side
@ManyToOne
@JoinColumn(name = "department_id")
private Department department;

// Inverse side
@OneToMany(mappedBy = "department")
private List<Employee> employees;

```
</details> 
<details> 
  <summary>⚡ Lazy vs Eager Loading 🚀</summary>
  
  ### ➡️ Meaning:
  | Feature    | Lazy           | Eager          |
|------------|----------------|----------------|
| Load Time  | When accessed  | Immediately     |
| Memory     | Saves memory   | Uses more       |
| Best For   | Large data      | Small relationships |

---

### Example:-

### 🔥 Java Code

```java
@OneToMany(fetch = FetchType.LAZY)
private List<Employee> employees;

- Employees are fetched only when accessed.

@OneToMany(fetch = FetchType.EAGER)
private List<Employee> employees;

- Employees are fetched immediately with Department.

```
</details>

### ✅ Conclusion
-- Now you can create any relationship!

-- You know how database tables and Java entities connect.

-- You understand lazy loading, eager loading, owning sides, and how JSON serialization works.





