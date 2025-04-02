Sure! Here's a structured workbook for your students to self-learn database management for an educational institution from beginning to end. I've arranged it logically, starting with basic concepts and progressing toward more advanced operations, as you requested.

---

## **Database Management for an Educational Institution: Self-Learning Workbook**

### **Table of Contents**

1. **Introduction to Database Management**
2. **Creating Tables and Defining Relationships**
3. **Basic Data Operations (DML)**
4. **Selecting and Filtering Data**
5. **Sorting and Limiting Data**
6. **Grouping and Aggregating Data**
7. **Joining Data from Multiple Tables**
8. **Subqueries and Correlated Subqueries**
9. **Using Functions and Custom Columns**
10. **Database Normalization**
11. **User Management and Security**
12. **Backup and Restore Operations**
13. **Constraints and Triggers**
14. **Summary and Conclusion**

---

### 1. **Introduction to Database Management**

In this section, you will learn about the basics of database management and the entities involved in the educational institution database.

#### **Entities**
- **Student**: Students who enroll in courses.
- **Lecturer**: Lecturers who teach courses.
- **Coordinator**: Coordinators who manage lecturers.
- **Branch**: The branch to which a coordinator belongs.
- **Course**: The courses available for students.

#### **Relationships**
- **Students** can take many **Courses**.
- **Lecturers** can teach many **Courses**.
- A **Course** can have multiple **Lecturers**.
- **Coordinators** manage multiple **Lecturers**.
- One **Branch** can have many **Coordinators**.

---

### 2. **Creating Tables and Defining Relationships**

You will now create the tables for your database and define the relationships between them.

#### **Create Tables**

```sql
CREATE TABLE branch (
    branch_id SERIAL PRIMARY KEY,
    branch_name VARCHAR(255) NOT NULL
);

CREATE TABLE coordinator (
    coordinator_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    branch_id INT REFERENCES branch(branch_id)
);

CREATE TABLE lecturer (
    lecturer_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    coordinator_id INT REFERENCES coordinator(coordinator_id)
);

CREATE TABLE course (
    course_id SERIAL PRIMARY KEY,
    course_name VARCHAR(255) NOT NULL
);

CREATE TABLE student (
    student_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    branch_id INT REFERENCES branch(branch_id),
    join_year INT NOT NULL
);

CREATE TABLE enrollment (
    student_id INT REFERENCES student(student_id),
    course_id INT REFERENCES course(course_id),
    PRIMARY KEY(student_id, course_id)
);

CREATE TABLE teaches (
    lecturer_id INT REFERENCES lecturer(lecturer_id),
    course_id INT REFERENCES course(course_id),
    PRIMARY KEY(lecturer_id, course_id)
);
```

#### **Insert Sample Data**

```sql
INSERT INTO branch (branch_name) VALUES ('Computer Science'), ('Business'), ('Engineering');
INSERT INTO coordinator (name, branch_id) VALUES ('Dr. Smith', 1), ('Dr. Brown', 2);
INSERT INTO lecturer (name, coordinator_id) VALUES ('John Doe', 1), ('Jane Doe', 2);
INSERT INTO course (course_name) VALUES ('Database Management'), ('Software Engineering');
INSERT INTO student (name, branch_id, join_year) VALUES ('Alice', 1, 2022), ('Bob', 2, 2023);
INSERT INTO enrollment (student_id, course_id) VALUES (1, 1), (2, 2);
INSERT INTO teaches (lecturer_id, course_id) VALUES (1, 1), (2, 2);
```

---

### 3. **Basic Data Operations (DML)**

In this section, we will cover the Data Manipulation Language (DML) operations to insert, update, delete, and manipulate data.

#### **INSERT (Add Data)**

```sql
INSERT INTO student (name, branch_id, join_year) VALUES ('Charlie', 3, 2024);
```

#### **DELETE (Remove Data)**

```sql
DELETE FROM student WHERE student_id = 3;
```

#### **TRUNCATE (Remove All Data)**

```sql
TRUNCATE TABLE course;
```

#### **UPDATE (Modify Data)**

```sql
UPDATE student SET join_year = 2025 WHERE student_id = 1;
```

---

### 4. **Selecting and Filtering Data**

Learn how to retrieve data using various filtering techniques.

#### **SELECT (Retrieve Data)**

```sql
SELECT * FROM student;
```

#### **WHERE (Filtering Data)**

```sql
SELECT * FROM student WHERE branch_id = 1;
```

#### **DISTINCT (Retrieve Unique Values)**

```sql
SELECT DISTINCT course_name FROM course;
```

#### **LIKE (Pattern Matching)**

```sql
SELECT * FROM lecturer WHERE name LIKE 'John%';
```

#### **IN (Filtering Based on Set of Values)**

```sql
SELECT * FROM student WHERE branch_id IN (1, 2, 3);
```

#### **BETWEEN (Range Filtering)**

```sql
SELECT * FROM student WHERE join_year BETWEEN 2020 AND 2023;
```

#### **AND/OR (Multiple Conditions)**

```sql
SELECT * FROM student WHERE branch_id = 1 AND join_year > 2020;
```

---

### 5. **Sorting and Limiting Data**

This section teaches you how to sort and limit the data returned in queries.

#### **ORDER BY (Sorting Data)**

```sql
SELECT * FROM student ORDER BY name ASC;
```

#### **LIMIT (Restricting Rows)**

```sql
SELECT * FROM student LIMIT 5;
```

#### **OFFSET (Skipping Rows)**

```sql
SELECT * FROM student LIMIT 5 OFFSET 10;
```

---

### 6. **Grouping and Aggregating Data**

In this section, we will explore how to group data and use aggregate functions.

#### **GROUP BY (Grouping Rows)**

```sql
SELECT branch_id, COUNT(*) FROM student GROUP BY branch_id;
```

#### **HAVING (Filtering Groups)**

```sql
SELECT branch_id, COUNT(*) FROM student GROUP BY branch_id HAVING COUNT(*) > 10;
```

#### **Aggregate Functions (MIN, MAX, AVG, SUM)**

```sql
SELECT course_id, AVG(student_count) FROM course GROUP BY course_id;
```

#### **COUNT (Counting Rows)**

```sql
SELECT COUNT(*) FROM lecturer;
```

---

### 7. **Joining Data from Multiple Tables**

Learn how to combine data from multiple tables using joins.

#### **INNER JOIN (Matching Rows)**

```sql
SELECT student.name, course.course_name
FROM student
INNER JOIN enrollment ON student.student_id = enrollment.student_id
INNER JOIN course ON enrollment.course_id = course.course_id;
```

#### **LEFT JOIN (All Left Table Rows)**

```sql
SELECT student.name, course.course_name
FROM student
LEFT JOIN enrollment ON student.student_id = enrollment.student_id
LEFT JOIN course ON enrollment.course_id = course.course_id;
```

#### **RIGHT JOIN (All Right Table Rows)**

```sql
SELECT course.course_name, student.name
FROM course
RIGHT JOIN enrollment ON course.course_id = enrollment.course_id
RIGHT JOIN student ON enrollment.student_id = student.student_id;
```

#### **FULL JOIN (All Matching or Non-matching Rows)**

```sql
SELECT student.name, course.course_name
FROM student
FULL JOIN enrollment ON student.student_id = enrollment.student_id
FULL JOIN course ON enrollment.course_id = course.course_id;
```

#### **UNION (Combining Results)**

```sql
SELECT name FROM student WHERE branch_id = 1
UNION
SELECT name FROM student WHERE branch_id = 2;
```

---

### 8. **Subqueries and Correlated Subqueries**

Learn how to use subqueries for more complex filtering.

#### **Subquery (Query within a Query)**

```sql
SELECT name FROM student WHERE student_id IN
(SELECT student_id FROM enrollment WHERE course_id IN
(SELECT course_id FROM course WHERE lecturer_id = (SELECT lecturer_id FROM lecturer WHERE name = 'John')));
```

#### **Correlated Subquery**

```sql
SELECT name FROM student WHERE student_id IN
(SELECT student_id FROM enrollment WHERE student_id = student.student_id GROUP BY student_id HAVING COUNT(*) > 3);
```

---

### 9. **Using Functions and Custom Columns**

In this section, you will learn how to use built-in functions and create custom columns.

#### **Custom Columns**

```sql
SELECT CONCAT(name, ' - ', branch_id) AS student_details FROM student;
```

#### **Built-in Functions**

```sql
SELECT LENGTH(name) FROM student;
```

#### **NOW() (Current Timestamp)**

```sql
SELECT NOW();
```

#### **COALESCE (Handling NULLs)**

```sql
SELECT COALESCE(course_name, 'N/A') FROM course;
```

---

### 10. **Database Normalization**

Understand how normalization reduces redundancy and dependency.

- **1NF (First Normal Form)**: Ensure each column contains atomic values.
- **2NF (Second Normal Form)**: Remove partial dependencies by ensuring each table has a primary key and no subsets of data depend on part of a composite key.
- **3NF (Third Normal Form)**: Remove transitive dependencies where non-key columns should depend only on the primary key.

---

### 11. **User Management and Security**

Learn how to manage database users and their permissions.

#### **Create Users and Grant Access**

```sql
CREATE USER admin WITH PASSWORD 'Admin123';
GRANT ALL PRIVILEGES ON DATABASE edu_institution TO admin;
```

#### **Grant, Revoke, and Deny Permissions**

```sql
GRANT SELECT, INSERT, UPDATE ON student TO admin;
REVOKE UPDATE ON student FROM student_user;
DENY DELETE ON student TO student_user;
```

---

### 12. **Backup and Restore Operations**

Learn how to back up and restore your database.

#### **Backup the Database**

```bash
pg_dump -U admin -F c -b -v -f edu_backup.sql edu_institution
```

#### **Restore the Database**

```bash
pg_restore -U admin -d edu_institution -v edu_backup.sql
```

---

### 13. **Constraints and Triggers**

Learn about enforcing data integrity with constraints and triggers.

#### **Constraints**

```sql
ALTER TABLE student ADD CONSTRAINT unique_student_name UNIQUE (name);
ALTER TABLE

 student ADD CONSTRAINT fk_branch FOREIGN KEY (branch_id) REFERENCES branch(branch_id);
```

#### **Triggers**

```sql
CREATE OR REPLACE FUNCTION log_enrollment() RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO enrollment_log (student_id, course_id)
    VALUES (NEW.student_id, NEW.course_id);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER enrollment_trigger
AFTER INSERT ON enrollment
FOR EACH ROW
EXECUTE FUNCTION log_enrollment();
```

---

### 14. **Summary and Conclusion**

This workbook covers the fundamentals of database management, from creating and managing tables to using advanced SQL features like joins, subqueries, and triggers. Mastery of these concepts will allow you to design, optimize, and maintain complex relational databases.

---

Good luck with your learning!
