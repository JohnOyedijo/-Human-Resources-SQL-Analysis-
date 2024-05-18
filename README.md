### DATA IN MOTION CASE STUDY 2: Human Resources

## Overview of the Project

This human resources project involves analyzing data from three tables:

- DEPARTMENTS
- EMPLOYEES
- PROJECTS

To address the business problems and generate valuable insights, I utilized several MySQL functions, including:

- Basic aggregations
- Window functions
- Date and Time functions
- Joins
- Group By and Order By

This approach enables efficient data analysis and uncovers key patterns and trends within the human resources domain.

## Business Problems and Solutions

1. **Find the longest ongoing project for each department**
   ```sql
   SELECT b.department_id, a.name, DATEDIFF(a.end_date, a.start_date) AS project_days
   FROM projects a
   JOIN employees b ON b.department_id = a.department_id
   GROUP BY a.name
   ORDER BY project_days DESC;
   ```

2. **Find all employees who are not managers**
   ```sql
   SELECT a.name, a.job_title, a.department_id 
   FROM departments b
   RIGHT JOIN employees a ON a.id = b.id
   WHERE manager_id IS NULL;
   ```

3. **Find all employees who have been hired after the start of a project in their department**
   ```sql
   SELECT e.name, e.hire_date 
   FROM employees e
   WHERE e.hire_date > 
   (SELECT p.start_date 
    FROM projects p 
    WHERE p.department_id = e.department_id);
   ```

4. **Rank employees within each department based on their hire date (earliest hire gets the highest rank)**
   ```sql
   SELECT p.name, q.name, p.hire_date,
   RANK() OVER(PARTITION BY p.department_id ORDER BY p.hire_date) AS ranking_per_department
   FROM departments q
   JOIN employees p ON q.id = p.department_id
   JOIN projects r ON p.department_id = r.department_id;
   ```

5. **Find the duration between the hire date of each employee and the hire date of the next employee hired in the same department**
   ```sql
   SELECT d.name, e.hire_duration_days
   FROM 
   (SELECT department_id, DATEDIFF(MAX(hire_date), MIN(hire_date)) AS hire_duration_days 
    FROM employees
    GROUP BY department_id) AS e
   INNER JOIN departments d ON e.department_id = d.id;
   ```

This structured approach helps in efficiently analyzing the data to uncover critical insights and trends within the human resources domain.
![SQL CODE 1](https://github.com/JohnOyedijo/-Human-Resources-SQL-Analysis-/assets/170008850/3060b8ec-00a1-462d-9ae6-843d4ed6f278)
![SQL CODE 2](https://github.com/JohnOyedijo/-Human-Resources-SQL-Analysis-/assets/170008850/4190c882-c224-4e6d-ba01-8995a0f37476)
![solution to Question 1](https://github.com/JohnOyedijo/-Human-Resources-SQL-Analysis-/assets/170008850/6b85edcf-d62e-4f67-9ad9-ef9c46a5c0a2)
![Solution to Question 2](https://github.com/JohnOyedijo/-Human-Resources-SQL-Analysis-/assets/170008850/29678f7a-f595-4a0e-9ffe-dd4600f1f0d7)
![Solution to Question 3](https://github.com/JohnOyedijo/-Human-Resources-SQL-Analysis-/assets/170008850/daed56de-e95a-43ea-97d8-50744ed4de21)
![Solution to Question 4](https://github.com/JohnOyedijo/-Human-Resources-SQL-Analysis-/assets/170008850/a85fb4bf-472a-4da8-804f-fd6b43730a61)
![Solution to Question 5](https://github.com/JohnOyedijo/-Human-Resources-SQL-Analysis-/assets/170008850/b98424fe-ec04-481e-86c0-1e4f4b5fd49f)
