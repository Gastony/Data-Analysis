--  Q-1. Write an SQL query to fetch "FIRST_NAME" from Worker table using the alias name as <WORKER_NAME>.

  Select FIRST_NAME AS WORKER_NAME from Worker;
 
--  Q-2. Write an SQL query to fetch "FIRST_NAME" from Worker table in upper case.
  Select upper(FIRST_NAME) from Worker;


-- Q-3. Write an SQL query to fetch unique values of DEPARTMENT from Worker table.

  Select distinct DEPARTMENT from Worker;

-- Q-4. Write an SQL query to print the first three characters of  FIRST_NAME from Worker table.

 Select substr(FIRST_NAME,1,3) from Worker;


-- Q-5. Write an SQL query to find the position of the alphabet
-- ('a') in the first name column 'Amitabh' from Worker table.

-- SELECT INSTR(FIRST_NAME, "a") FROM worker WHERE FIRST_NAME = "Amitabh";
 SELECT POSITION('A' IN FIRST_NAME) AS position FROM worker WHERE FIRST_NAME = 'Amitabh'; --postgresql


--  Q-6. Write an SQL query to print the FIRST_NAME from Worker
--  table after removing white spaces from the right side.

  SELECT rtrim(FIRST_NAME) from Worker;
 
--  Q-7. Write an SQL query to print the DEPARTMENT from Worker
--  table after removing white spaces from the left side.

 SELECT ltrim(DEPARTMENT) from Worker;
 
--  Q-8. Write an SQL query that fetches the unique values of DEPARTMENT 
-- from Worker  table and prints its length.
 
  SELECT DISTINCT DEPARTMENT, LENGTH(DEPARTMENT) AS department_length FROM Worker;
 
-- Q-9. Write an SQL query to print the FIRST_NAME from Worker 
-- table after replacing 'a' with 'A'.

--  SELECT replace(FIRST_NAME, 'a', 'A') from Worker;
 
--  Q-10. Write an SQL query to print the FIRST_NAME and LAST_NAME from Worker table into a single 
--  column COMPLETE_NAME. A space char should separate them.
 
--  SELECT FIRST_NAME || ' '|| LAST_NAME as COMPLETE_NAME from Worker;

-- Q-11 Write an SQL to Print all Worker details from the Worker table ordered by FIRST_NAME Ascending.
SELECT * FROM worker ORDER BY first_name;

-- Q-12 Write an SQL to Print all Worker details from the Worker table ordered by FIRST_NAME Ascending and DEPARTMENT Descending.
SELECT * FROM worker ORDER BY first_name, department DESC;
 
 -- Q-13 Write an SQL to Print details for Workers with the first names "Vipul" and "Satish" from the Worker table.
 SELECT * FROM worker WHERE first_name IN ('Vipul', 'Satish');


-- Q-14 Write an SQL to Print details of workers excluding first names, "Vipul" and "Satish" from the Worker table.
 SELECT * FROM worker WHERE first_name NOT IN ('Vipul', 'Satish');


-- Q-15 Write an SQL to Print details of Workers with DEPARTMENT name starting with "Admin".
 SELECT * FROM worker WHERE department LIKE 'Admin%';


-- Q-16 Write an SQL to Print details of the Workers whose FIRST_NAME contains 'a'.
 SELECT * FROM worker WHERE first_name LIKE '%a%';



-- Q-17 Write an SQL to Print details of the Workers whose FIRST_NAME ends with 'a'.
 SELECT * FROM worker WHERE first_name LIKE '%a';


-- Q-18 Write an SQL to Print details of the Workers whose FIRST_NAME ends with 'h' and contains six alphabets.
SELECT * FROM worker WHERE first_name LIKE '_____h';

-- Q-19 Write an SQL to Print details of the Workers whose SALARY lies between 100,000 and 500,000.
 SELECT * FROM worker WHERE salary BETWEEN 100000 AND 500000;


-- Q-20 Write an SQL to Print details of the Workers who have joined in Feb 2014.
 ---SELECT * FROM worker WHERE YEAR(joining_date) = 2014 AND MONTH(joining_date) = 02;

 SELECT * FROM worker WHERE EXTRACT(YEAR FROM joining_date) = 2014 AND EXTRACT(MONTH FROM joining_date) = 2;


 
--  Q-21. Write an SQL query to fetch the count of employees working in the
--  department 'Admin'.
 
  SELECT count(*) from Worker where department='Admin';
 
--  Q-22. Write an SQL query to fetch worker names with salaries >= 
--  50000 and <= 100000.

 SELECT concat(FIRST_NAME,' ', LAST_NAME) as WORKER_NAME FROM worker where SALARY>=50000 and SALARY<=100000;
 

-- Q-23. Write an SQL query to fetch the no. of workers 
-- for each department in the descending order.

 SELECT DEPARTMENT, count(WORKER_ID) NO_Of_Wokreser  FROM worker Group by DEPARTMENT  order by NO_Of_Wokreser desc;


-- Q-24. Write an SQL query to print details of the Workers 
-- who are also Managers.

SELECT W.FIRST_NAME, T.WORKER_TITLE FROM worker W INNER JOIN Title T on W.WORKER_ID=T.WORKER_REF_ID and T.WORKER_TITLE in('Manager');

-- Q-25. Write an SQL query to fetch duplicate records having matching data 
-- in some fields of a table.
 SELECT WORKER_TITLE, AFFECTED_FROM, count(*) FROM Title Group by WORKER_TITLE, AFFECTED_FROM having count(*)>1;

-- Q-26. Write an SQL query to show only odd rows from a table.

 SELECT * FROM worker WHERE worker_id % 2 <> 0;


-- Q-27. Write an SQL query to Show only even rows from a table.
 SELECT * FROM worker WHERE MOD(WORKER_ID, 2) = 0;

-- Q-28. Write an SQL query to Clone a new table from another table.
 -- Create a new table with the same structure as the existing table
CREATE TABLE worker_clone AS TABLE worker WITH NO DATA;

-- Copy data from the existing table to the new table
INSERT INTO worker_clone SELECT * FROM worker;

-- Q-29. Write an SQL query to Fetch intersecting records of two tables.
 SELECT worker.* FROM worker INNER JOIN worker_clone USING(worker_id);

-- Q-30. Write an SQL query to show records from one table 
-- that another table does not have.

SELECT FIRST_NAME, WORKER_ID FROM WORKER  WHERE NOT EXISTS (SELECT * FROM Title WHERE WORKER.WORKER_ID=Title.WORKER_REF_ID);


-- Q-31. Write an SQL query to Show the top n (say 5) records of a table ordered by descending salary.
 SELECT * FROM worker ORDER BY salary DESC LIMIT 5;


-- Q-32. Write an SQL query to Determine the nth (say n=5) highest salary from a table.
SELECT * FROM worker ORDER BY salary DESC LIMIT 4,1;

-- Q-33. Write an SQL query to Determine the 5th highest salary without using the LIMIT keyword.
 SELECT salary FROM worker w1 WHERE 4 = ( SELECT COUNT(DISTINCT (w2.salary)) from worker w2 where w2.salary >= w1.salary );

-- Q-34. Write an SQL query to Fetch the list of employees with the same salary.
 SELECT w1.* from worker w1, worker w2 where w1.salary = w2.salary and w1.worker_id != w2.worker_id;

-- Q-35. Write an SQL query to Show the second-highest salary from a table using a sub-query.
 SELECT max(salary) from worker where salary not in (select max(salary) from worker);

-- Q-36. Write an SQL query to List worker_id who does not get a bonus.
 SELECT worker_id from worker where worker_id not in (select worker_ref_id from bonus);

-- Q-37. Write an SQL query to Fetch the departments that have less than 4 people in it.
 SELECT department, count(department) as depCount from worker group by department having depCount < 4;

-- Q-38. Write an SQL query to Show all departments along with the number of people in there.
 SELECT department, count(department) as depCount from worker group by department;


-- Q-39. Write an SQL query to Show the last record from a table.
 SELECT * from worker where worker_id = (select max(worker_id) from worker);

-- Q-40. Write an SQL query to Fetch the last five records from a table.
SELECT *
FROM (
    SELECT *
    FROM worker
    ORDER BY worker_id DESC
    LIMIT 5
) AS subquery
ORDER BY worker_id;


-- Q-41. Write an SQL query to Print the employees with the highest salary in each department.
 SELECT w.department, w.first_name, w.salary from (select max(salary) as maxsal, department from worker group by department) temp inner join worker w on temp.department = w.department and temp.maxsal = w.salary;


-- Q-42. Write an SQL query to  Fetch three max salaries from a table using a co-related subquery.
 SELECT distinct salary from worker w1 where 3 >= (select count(distinct salary) from worker w2 where w1.salary <= w2.salary) order by w1.salary desc;


-- Q-43. Write an SQL query to  Fetch nth max salaries from a table (3 rd max salary).
 SELECT distinct salary from worker w1 where 3 >= (select count(distinct salary) from worker w2 where w1.salary <= w2.salary) order by w1.salary desc;


-- Q-44. Write an SQL query to   Fetch departments along with the total salaries paid for each of them.
 SELECT department, sum(salary) as depSal from worker group by department order by depSal desc;


-- Q-45. Write an SQL query to   Fetch the names of workers who earn the highest salary.
 SELECT first_name, salary from worker where salary = (select max(Salary) from worker)


-- Q-46. Write an SQL query to print the name of employees 
-- having the highest salary in each department.

SELECT t.DEPARTMENT,t.FIRST_NAME,t.Salary
FROM(SELECT max(Salary) as TotalSalary, DEPARTMENT FROM WORKER Group by 
DEPARTMENT) as TempNew
INNER JOIN WORKER t on TempNew.DEPARTMENT=t.DEPARTMENT
and TempNew.TotalSalary=t.Salary;


-- Q-47. Write an SQL query to fetch three min salaries from a table.

select distinct Salary FROM WORKER a where 3>=
(select count(distinct Salary) FROM WORKER b
 where a.Salary<=b.Salary) order by a.Salary desc;



-- Q-48. Write an SQL query to fetch nth max salaries from a table.

 SELECT distinct Salary from worker a WHERE 2 >= 
 (SELECT count(distinct Salary) from worker b WHERE a.Salary <= b.Salary) 
 order by a.Salary desc;


-- Q-49. Write an SQL query to fetch departments along with the total salaries 
-- paid for each of them.

SELECT DEPARTMENT, sum(Salary) FROM worker group by DEPARTMENT;

--Q-50. Write An SQL Query To Fetch The Names Of Workers Who Earn The Highest Salary.

SELECT FIRST_NAME, SALARY from Worker WHERE SALARY=(SELECT max(SALARY) from Worker);