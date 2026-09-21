

## Syntax 

```sql
SELECT columns
FROM TableA
JOIN TableB
    ON TableA.SomeColumn = TableB.SomeColumn;
```

### `ON`
Defines **how the tables are related**.

```sql
ON Students.ClassId = Classes.Id
```

### `SELECT`
Defines **what information you want**.

```sql
SELECT Students.Name, Classes.Name
```

---

## [Sahad] Example

```sql
select 
	Users.FirstName,
	Users.LastName,
	StudentUserProfiles.SchoolName
from Users
JOIN StudentUserProfiles
	On StudentUserProfiles.UserId = Users.Id;
```

---

## Example

```sql
SELECT
    Students.Name,
    Classes.Name
FROM Students
JOIN Classes
    ON Students.ClassId = Classes.Id;
```

| Student | Class            |
| ------- | ---------------- |
| Ali     | Mathematics      |
| Reza    | Physics          |
| Sara    | Mathematics      |
| Hadi    | Computer Science |

---

## `INNER JOIN`


When you write:
```sql
JOIN
```

SQL Server treats it as:
```sql
INNER JOIN
```


So these are equivalent:
```sql
SELECT *
FROM Students
JOIN Classes
    ON Students.ClassId = Classes.Id;
```

and:
```sql
SELECT *
FROM Students
INNER JOIN Classes
    ON Students.ClassId = Classes.Id;
```


An `INNER JOIN` means:

> Give me only rows where there is a match in **both** tables


---

## `LEFT JOIN`


```sql
SELECT
    Students.Name,
    Classes.Name
FROM Students
LEFT JOIN Classes
    ON Students.ClassId = Classes.Id;
```

`LEFT JOIN` means:

> Keep **every row from the left table**, even if there is no match on the right.

### Students

| Id  | Name | ClassId |
| --- | ---- | ------- |
| 1   | Ali  | 10      |
| 2   | Reza | 20      |
| 3   | Sara | 10      |
| 4   | Hadi | 999     |

### Result:

| Student | Class       |
| ------- | ----------- |
| Ali     | Mathematics |
| Reza    | Physics     |
| Sara    | Mathematics |
| Hadi    | NULL        |


```
INNER JOIN
     ↓
Only matching rows

LEFT JOIN
     ↓
Everything from LEFT + matching data from RIGHT
```

---

## `RIGHT JOIN`

`RIGHT JOIN` is basically the opposite of `LEFT JOIN`.

```sql
SELECT
    Students.Name,
    Classes.Name
FROM Students
RIGHT JOIN Classes
    ON Students.ClassId = Classes.Id;
```

This means:

> Keep every row from the **right table**.


In practice, I recommend mostly thinking in terms of `LEFT JOIN` rather than `RIGHT JOIN`.

---

## `FULL OUTER JOIN`

This one keeps **everything from both tables**.

```sql
SELECT
    Students.Name,
    Classes.Name
FROM Students
FULL OUTER JOIN Classes
    ON Students.ClassId = Classes.Id;
```

So if:
- Hadi has `ClassId = 999`, which doesn't exist
- Chemistry has no students

both remain.

---

## `CROSS JOIN`

It produces **every possible combination**.


### Colors

|Color|
|---|
|Red|
|Blue|

### Sizes

|Size|
|---|
|S|
|M|
|L|


```sql
SELECT *
FROM Colors
CROSS JOIN Sizes;
```

### Result:

| Color | Size |
| ----- | ---- |
| Red   | S    |
| Red   | M    |
| Red   | L    |
| Blue  | S    |
| Blue  | M    |
| Blue  | L    |


You don't specify an `ON` condition.


If one of table has no rows (like Sizes table is empty) result is not going to have any rows.


`RESULT row count = Sizes row count * Colors row count`


---

## `JOIN more than two tables`


### `Students`

```
Id
Name
```

### `Enrollments`

```
StudentId
CourseId
```

### `Courses`

```
Id
Name
```


So this is like:
`Stuents <- Enrollments -> Courses`


You want:

> Student name + course name


```sql
SELECT
    Students.Name AS StudentName,
    Courses.Name AS CourseName
FROM Students
JOIN Enrollments
    ON Enrollments.StudentId = Students.Id
JOIN Courses
    ON Enrollments.CourseId = Courses.Id;
```


You can keep adding JOINs:

```sql
FROM A
JOIN B ON ...
JOIN C ON ...
JOIN D ON ...
JOIN E ON ...
```


---


## Table aliases

Instead of:
```sql
SELECT
    Students.Name,
    Classes.Name
FROM Students
JOIN Classes
    ON Students.ClassId = Classes.Id;
```

you can write:
```sql
SELECT
    s.Name,
    c.Name
FROM Students AS s
JOIN Classes AS c
    ON s.ClassId = c.Id;
```

or simply:
```sql
FROM Students s
JOIN Classes c
```


---


# JOIN and one-to-many relationships

This gives us a useful mental model.

### One-to-one

```
User ───── Profile
  1          1
```

A user has one profile.

### One-to-many

```
Student ─────< Exam
    1          many
```

One student has many exams.

### Many-to-many

```
Student >──── Enrollment ────< Course
```

One student can have many courses, and one course can have many students.

That's why we need the middle table:

```
Students
   ↓
Enrollments
   ↓
Courses
```

Understanding database relationships makes JOINs much easier.

---


## `JOIN + WHERE`

You can combine JOIN with filtering:
```sql
SELECT
    s.Name,
    c.Name
FROM Students s
JOIN Classes c
    ON s.ClassId = c.Id
WHERE c.Name = 'Mathematics';
```


```
JOIN the tables
      ↓
Filter the result
      ↓
Return selected columns
```


---


## `ON` vs `WHERE`


Consider:
```sql
SELECT *
FROM Students s
LEFT JOIN Classes c
    ON s.ClassId = c.Id
WHERE c.Name = 'Mathematics';
```

This can behave very differently from:
```sql
SELECT *
FROM Students s
LEFT JOIN Classes c
    ON s.ClassId = c.Id
    AND c.Name = 'Mathematics';
```


In the first:
```sql
WHERE c.Name = 'Mathematics'
```

rows where `c` is `NULL` are removed.

So you've effectively made the LEFT JOIN behave much more like an INNER JOIN for that condition.

In the second:

```sql
ON s.ClassId = c.Id
AND c.Name = 'Mathematics'
```

you're saying:

> "Join a class only if it is Mathematics, but still keep students that don't have such a class."

This distinction is **very important** once you start writing complicated queries.

---

## A practical LMS example

Let's make this closer to the kind of SQL you're working with.

Imagine:

```
Users
-----
Id
FirstName
LastName

Courses
-------
Id
Title
TeacherId

CourseStudents
--------------
CourseId
StudentId
```

We want:

> Show every course with its teacher's name.

```
SELECT
    c.Title,
    u.FirstName,
    u.LastName
FROM Courses c
JOIN Users u
    ON c.TeacherId = u.Id;
```

Now:

> Show every student enrolled in every course.

```
SELECT
    u.FirstName,
    u.LastName,
    c.Title
FROM CourseStudents cs
JOIN Users u
    ON cs.StudentId = u.Id
JOIN Courses c
    ON cs.CourseId = c.Id;
```

And:

> Show every course and how many students are enrolled.

```
SELECT
    c.Id,
    c.Title,
    COUNT(cs.StudentId) AS StudentCount
FROM Courses c
LEFT JOIN CourseStudents cs
    ON c.Id = cs.CourseId
GROUP BY
    c.Id,
    c.Title;
```

The `LEFT JOIN` is important here.

It allows a course with **zero students** to still appear:

| Course       | StudentCount |
| ------------ | ------------ |
| Mathematics  | 31           |
| Physics      | 18           |
| Empty Course | 0            |

With an `INNER JOIN`, `Empty Course` would disappear.

---

## Mental model


When you see:
```sql
FROM A
JOIN B
    ON A.X = B.Y
```

read it in English:

> "Start with A. Connect it to B where A.X equals B.Y."



For:
```
LEFT JOIN
```

read:

> "Keep everything from A, and attach B where possible."



For:
```
INNER JOIN
```

read:

> "Keep only rows where the connection exists."



For:
```
RIGHT JOIN
```

read:

> "Keep everything from B."



For:
```
FULL OUTER JOIN
```

read:

> "Keep everything from both sides."


---


## Cheat sheet


|JOIN|What it keeps|
|---|---|
|`INNER JOIN`|Matching rows only|
|`LEFT JOIN`|Everything from left + matches from right|
|`RIGHT JOIN`|Everything from right + matches from left|
|`FULL OUTER JOIN`|Everything from both|
|`CROSS JOIN`|Every possible combination|