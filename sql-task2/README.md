# SQL PRACTICE
### SQL Practice №2 | The Departments

Relational Schema

![sql2](https://user-images.githubusercontent.com/90597917/206886341-93cb1c19-f1f9-41fd-b525-ed4e0dbdd62d.png)
#### Table creation code

```sql
CREATE TABLE Departments2
(
    Code   INTEGER PRIMARY KEY NOT NULL,
    Name   VARCHAR             NOT NULL,
    Budget REAL                NOT NULL
);

CREATE TABLE Employees2
(
    SSN        INTEGER PRIMARY KEY NOT NULL,
    Name       TEXT                NOT NULL,
    LastName   VARCHAR             NOT NULL,
    Department INTEGER             NOT NULL,
    CONSTRAINT fk_Departments_Code FOREIGN KEY (Department)
        REFERENCES Departments2 (Code)
);
```
#### Sample dataset

```sql
INSERT INTO Departments2(Code, Name, Budget)
VALUES(14, 'IT', 65000),
      (37, 'Accounting', 15000),
      (59, 'Human Resources', 240000),
      (77, 'Research', 55000);

INSERT INTO Employees2(SSN, Name, LastName, Department)
VALUES('123234877', 'Michael', 'Rogers', 14),
      ('152934485', 'Anand', 'Manikutty', 14),
      ('222364883', 'Carol', 'Smith', 37),
      ('326587417', 'Joe', 'Stevens', 37),
      ('332154719', 'Mary-Anne', 'Foster', 14),
      ('332569843', 'George', 'O''Donnell', 77),
      ('546523478', 'John', 'Doe', 59),
      ('631231482', 'David', 'Smith', 77),
      ('654873219', 'Zacary', 'Efron', 59),
      ('745685214', 'Eric', 'Goldsmith', 59),
      ('845657245', 'Elizabeth', 'Doe', 14),
      ('845657246', 'Kumar', 'Swamy', 14);
 ```
      
### Exercises

1. Select the last name of all employees.

2. Select the last name of all employees, without duplicates.

3. Select all the data of employees whose last name is "Smith".

4. Select all the data of employees whose last name is "Smith" or "Doe".

5. Select all the data of employees that work in department 14.

6. Select all the data of employees that work in department 37 or department 77.

7. Select all the data of employees whose last name begins with an "S".

8. Select the sum of all the departments' budgets.

9. Select the number of employees in each department (you only need to show the department code and the number of employees).

10. Select all the data of employees, including each employee's department's data.

11. Select the name and last name of each employee, along with the name and budget of the employee's department.

12. Select the name and last name of employees working for departments with a budget greater than $60,000.

13. Select the departments with a budget larger than the average budget of all the departments.

14. Select the names of departments with more than two employees.

15. Select the name and last name of employees working for departments with second lowest budget.

16. Add a new department called "Quality Assurance", with a budget of $40,000 and departmental code 11. Add an employee called "Mary Moore" in that department, with SSN 847-21-9811.

17. Reduce the budget of all departments by 10%.

18. Reassign all employees from the Research department (code 77) to the IT department (code 14).

19. Delete from the table all employees in the IT department (code 14).

20. Delete from the table all employees who work in departments with a budget greater than or equal to $60,000.

21. Delete from the table all employees.

#### Answers

```sql
1. SELECT lastname
   FROM employees2;

2. SELECT DISTINCT lastname
   FROM employees2;

3. SELECT *
   FROM employees2
   WHERE lastname = 'Smith';

4. SELECT *
   FROM employees2
   WHERE lastname IN ('Smith', 'Doe');

5. SELECT *
   FROM employees2
   WHERE department = 14;

6. SELECT *
   FROM employees2
   WHERE department = 37
   OR department = 77;

7. SELECT *
   FROM employees2
   WHERE lastname LIKE 'S%';

8. SELECT SUM(budget) AS sum
   FROM departments2;

9. SELECT department, count(*)
   FROM employees2,
        departments2
   WHERE employees2.department = departments2.code
   GROUP BY employees2.department;
    
10. SELECT *
    FROM employees2 e
         INNER JOIN departments2 d
                    ON e.department = d.Code;
   
11. SELECT e.name AS name, e.lastname, d.name AS department, d.budget
    FROM employees2 e
         INNER JOIN departments2 d on d.code = e.department;

12. SELECT e.name, e.lastname
    FROM employees2 e
         INNER JOIN departments2 d on d.code = e.department
    WHERE d.budget > 60000;

13. SELECT name
    FROM departments2
    WHERE budget > (SELECT AVG(budget) FROM departments2);

14. SELECT d.name
    FROM departments2 d
         JOIN employees2 e on d.code = e.department
    GROUP BY d.name
    HAVING count(*) > 2;

15. SELECT e.name, e.lastname
    FROM employees2 e
    WHERE department = (
    SELECT code
    FROM (SELECT * FROM departments2 d ORDER BY d.budget LIMIT 2) sub
    ORDER BY budget DESC
    LIMIT 1);

16. INSERT INTO departments2(code, name, budget)
    VALUES (11, 'Quality Assurance', 40000);
    
    INSERT INTO employees2(ssn, name, lastname, department)
    VALUES (847219811, 'Mary', 'Moore', 11);
    
17. UPDATE departments2
    SET budget = budget - (budget * 0.1);
    
18. UPDATE employees2
    SET department = 14
    WHERE department = 77;
    
19. DELETE
    FROM employees2
    WHERE department = 14;
    
20. DELETE
    FROM employees2
    WHERE department IN
      (
          SELECT code
          FROM departments2
          WHERE budget >= 60000
      );
    
21. DELETE
    FROM employees2;
