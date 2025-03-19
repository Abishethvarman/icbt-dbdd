### Document: Teaching Database Management for an Educational Institution

#### 1. **Overview of the Educational Institution Database**
We are creating a database management system (DBMS) for an educational institution with the following entities:
- **Student**: Students who enroll in courses.
- **Lecturer**: Lecturers who teach courses.
- **Coordinator**: Coordinators who manage lecturers.
- **Branch**: The branch to which a coordinator belongs.
- **Course**: The courses that are available for students and taught by lecturers.

Relationships:
- **Students** can take many **Courses**.
- **Lecturers** can teach many **Courses**.
- A **Course** can have multiple **Lecturers**, but for each subject, there can be multiple lecturers.
- **Coordinators** manage multiple **Lecturers** but a **Lecturer** can only be managed by one **Coordinator**.
- One **Branch** can have many **Coordinators**.

---

### 2. **Basic Operations: Data Selection and Filtering**

1. **SELECT (Retrieve Data)**
   - Query to select all data from a table:
   ```sql
   SELECT * FROM student;
   ```

2. **WHERE (Filtering Data)**
   - Query to select students who belong to a specific branch:
   ```sql
   SELECT * FROM student WHERE branch_id = 1;
   ```

3. **DISTINCT (Retrieve Unique Values)**
   - Query to get distinct course names:
   ```sql
   SELECT DISTINCT course_name FROM course;
   ```

4. **LIKE (Pattern Matching)**
   - Query to find lecturers whose names start with "John":
   ```sql
   SELECT * FROM lecturer WHERE name LIKE 'John%';
   ```

5. **IN (Filtering Based on Set of Values)**
   - Query to select students from specific branches:
   ```sql
   SELECT * FROM student WHERE branch_id IN (1, 2, 3);
   ```

6. **NOT IN (Excluding Rows)**
   - Query to exclude students from certain branches:
   ```sql
   SELECT * FROM student WHERE branch_id NOT IN (4, 5);
   ```

7. **BETWEEN (Range Filtering)**
   - Query to find students who joined between two specific years:
   ```sql
   SELECT * FROM student WHERE join_year BETWEEN 2020 AND 2023;
   ```

8. **AND/OR (Multiple Conditions)**
   - Query to find students from branch 1 who joined after 2020:
   ```sql
   SELECT * FROM student WHERE branch_id = 1 AND join_year > 2020;
   ```

---

### 3. **Data Operations: DML (Data Manipulation Language)**

1. **INSERT (Add Data)**
   - Insert a new student:
   ```sql
   INSERT INTO student (name, branch_id, join_year) VALUES ('Alice', 1, 2022);
   ```

2. **DELETE (Remove Data)**
   - Delete a specific student:
   ```sql
   DELETE FROM student WHERE student_id = 5;
   ```

3. **TRUNCATE (Remove All Data)**
   - Remove all records from a table (faster than DELETE):
   ```sql
   TRUNCATE TABLE course;
   ```

4. **UPDATE (Modify Data)**
   - Update the year of a specific student:
   ```sql
   UPDATE student SET join_year = 2023 WHERE student_id = 1;
   ```

---

### 4. **Sorting and Limits**

1. **ORDER BY (Sorting Data)**
   - Query to sort students by name:
   ```sql
   SELECT * FROM student ORDER BY name ASC;
   ```

2. **LIMIT (Restricting Rows)**
   - Query to limit the result to 5 students:
   ```sql
   SELECT * FROM student LIMIT 5;
   ```

3. **OFFSET (Skipping Rows)**
   - Query to skip the first 10 students and return the next 5:
   ```sql
   SELECT * FROM student LIMIT 5 OFFSET 10;
   ```

---

### 5. **Grouping and Aggregating Data**

1. **GROUP BY (Grouping Rows)**
   - Query to count the number of students in each branch:
   ```sql
   SELECT branch_id, COUNT(*) FROM student GROUP BY branch_id;
   ```

2. **HAVING (Filtering Groups)**
   - Query to find branches with more than 10 students:
   ```sql
   SELECT branch_id, COUNT(*) FROM student GROUP BY branch_id HAVING COUNT(*) > 10;
   ```

3. **Aggregate Functions (MIN, MAX, AVG, SUM)**
   - Query to find the average number of students per course:
   ```sql
   SELECT course_id, AVG(student_count) FROM course GROUP BY course_id;
   ```

4. **COUNT (Counting Rows)**
   - Query to count how many lecturers are teaching:
   ```sql
   SELECT COUNT(*) FROM lecturer;
   ```

---

### 6. **Joins and Combining Data**

1. **INNER JOIN (Matching Rows)**
   - Query to list students with the courses they are enrolled in:
   ```sql
   SELECT student.name, course.course_name
   FROM student
   INNER JOIN enrollment ON student.student_id = enrollment.student_id
   INNER JOIN course ON enrollment.course_id = course.course_id;
   ```

2. **LEFT JOIN (All Left Table Rows)**
   - Query to find all students and the courses they are enrolled in (if any):
   ```sql
   SELECT student.name, course.course_name
   FROM student
   LEFT JOIN enrollment ON student.student_id = enrollment.student_id
   LEFT JOIN course ON enrollment.course_id = course.course_id;
   ```

3. **RIGHT JOIN (All Right Table Rows)**
   - Query to find all courses and the students enrolled in them:
   ```sql
   SELECT course.course_name, student.name
   FROM course
   RIGHT JOIN enrollment ON course.course_id = enrollment.course_id
   RIGHT JOIN student ON enrollment.student_id = student.student_id;
   ```

4. **FULL JOIN (All Matching or Non-matching Rows)**
   - Query to list all students and all courses, regardless of whether they are enrolled in a course:
   ```sql
   SELECT student.name, course.course_name
   FROM student
   FULL JOIN enrollment ON student.student_id = enrollment.student_id
   FULL JOIN course ON enrollment.course_id = course.course_id;
   ```

5. **UNION (Combining Results)**
   - Query to combine students from two different branches:
   ```sql
   SELECT name FROM student WHERE branch_id = 1
   UNION
   SELECT name FROM student WHERE branch_id = 2;
   ```

---

### 7. **Subqueries**

1. **Subquery (Query within a Query)**
   - Find students enrolled in courses taught by "John":
   ```sql
   SELECT name FROM student WHERE student_id IN
   (SELECT student_id FROM enrollment WHERE course_id IN
   (SELECT course_id FROM course WHERE lecturer_id = (SELECT lecturer_id FROM lecturer WHERE name = 'John')));
   ```

2. **Correlated Subquery**
   - Find students who have more than 3 enrollments:
   ```sql
   SELECT name FROM student WHERE student_id IN
   (SELECT student_id FROM enrollment WHERE student_id = student.student_id GROUP BY student_id HAVING COUNT(*) > 3);
   ```

---

### 8. **Functions and Custom Columns**

1. **Custom Columns**
   - Concatenate student name and branch:
   ```sql
   SELECT CONCAT(name, ' - ', branch_id) AS student_details FROM student;
   ```

2. **Built-in Functions**
   - Get the length of a student's name:
   ```sql
   SELECT LENGTH(name) FROM student;
   ```

3. **NOW() (Current Timestamp)**
   - Get the current timestamp:
   ```sql
   SELECT NOW();
   ```

4. **COALESCE (Handling NULLs)**
   - Show 'N/A' if course name is NULL:
   ```sql
   SELECT COALESCE(course_name, 'N/A') FROM course;
   ```

---

### 9. **Dropping Tables and Constraints**

1. **DROP (Delete Table or Constraint)**
   - Drop a table:
   ```sql
   DROP TABLE course;
   ```

   - Drop a constraint from a table:
   ```sql
   ALTER TABLE course DROP CONSTRAINT course_fk;
   ```

2. **ALTER (Modify Table)**
   - Add a new column to the student table:
   ```sql
   ALTER TABLE student ADD date_of_birth DATE;
   ```

---

### Suggested Order for Teaching:

1. **Data Selection and Filtering (Basic Operations)**
   - Start with basic queries: SELECT, WHERE, DISTINCT, LIKE, IN, BETWEEN, AND/OR.
   
2. **Data Operations (DML)**
   - Teach INSERT, DELETE, TRUNCATE, UPDATE to manipulate data.

3. **Sorting and Limits**
   - Introduce ORDER BY, LIMIT, and OFFSET for customization.

4. **Grouping and Aggregating Data**
   - Discuss GROUP BY, aggregate functions (MIN/MAX/COUNT/SUM/AVG), and HAVING.

5. **Joins and Combining Data**
   - Focus on JOIN operations and then UNION/UNION ALL.

6. **Subqueries**
   - Teach subqueries and correlated subqueries for advanced filtering.

7. **Functions and Custom Columns**
   - Introduce built-in SQL functions and custom columns for flexible results.

8. **Dropping Tables and Constraints**
   - Conclude with schema management: DROP and ALTER operations.

This structure should provide a solid foundation for understanding database queries and operations for an educational institution database.
