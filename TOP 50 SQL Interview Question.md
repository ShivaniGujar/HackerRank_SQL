# 1.  Find Second Highest Salary of an Employee
**Explanation**:



Here first we are creating resultset  excluding first highest salary using i.e max(salary),
again applying max(salary) on this result would fetch second highest salary

```sql
/*Approach 1: Subquery Approch with NOT IN*/

SELECT max(sal) FROM employee
WHERE sal NOT IN (SELECT max(sal) FROM employee);

/*Approach 2 : Subquery with < operator*/

 SELECT max(sal) FROM emp
   WHERE sal <(SELECT max(sal) FROM employee);
 ```

***Optimizes Approach***

Instead of using MAX(), you can use ORDER BY with LIMIT or OFFSET (if supported by your SQL database).



```sql
/*Approach 1: Using LIMIT with OFFSET (MySQL, PostgreSQL)  ,
The OFFSET clause tells SQL to skip a certain number of rows before starting to return results.
LIMIT 1 OFFSET 1	Returns the second highest salary.
LIMIT 1 OFFSET 2	Returns the third highest salary.*/

SELECT DISTINCT sal 
FROM employee
ORDER BY sal DESC 
LIMIT 1 OFFSET 1;

```


# 2. Display the highest paid employee in each department ?

**Explanation**:

we need to group employees by department and find the maximum salary within each department.

```sql
/*when using GROUP by clause then not allow to use any other column in select
ONLY suppose to use aggregate function followed by column that seggreagating*/

SELECT  MAX(sal),dept_id,
FROM employee 
GROUP BY dept_id;

/* Display the lowest paid employee in each department ?*/

SELECT min(sal),dept_id
FROM employee
GROUP BY dept_id;

/*Find no of employee present in each department */


SELECT COUNT(ename),dept_id
FROM employee
GROUP BY dept_id;

```

# 3. Display alternate records in SQL

**Explanation**:

Either you can explain Even or Odd numbers of record.
**pseudocolumn** , does not have physical storage but ***assign nd read only*** function behaves like a table column, but is not actually stored in the table. 
You can select from pseudocolumns, but you cannot insert, update, or delete their values.

1️⃣ What is ROWNUM?
Assign sequential number satring from 1 to all records and it cannot used with * aastreik
ROWNUM is a pseudocolumn in Oracle SQL that assigns a unique row number before sorting the data.
It does not store values physically in the table.
It is read-only and automatically generated when the query is executed.

```sql

/* TO diplay odd records*/

SELECT * FROM (
    SELECT empno, ename, sal, ROWNUM AS rn
    FROM employee 
    ORDER BY empno  -- Ensure a consistent order
) 
WHERE MOD(rn, 2) != 0; -- Select odd-numbered rows

/* TO diplay Even records*/

SELECT * FROM (
    SELECT empno, ename, sal, ROWNUM AS rn
    FROM employee 
    ORDER BY empno  -- Ensure a consistent order
) 
WHERE MOD(rn, 2) = 0; -- Select even-numbered rows


```

# 4. Find duplicate values and it's frequency of a column

**Explanation**:
frequency means how many times it appear so use count() for that, seggreagte the data on ename so use GROUP BY clause 
SELECT ename, COUNT(*) -->Selects the ename column (employee name) and counts the occurrences of each name in the emp table.
FROM emp-->Specifies the table from which data is retrieved.
GROUP BY ename-->Groups the records by the ename column, meaning all rows with the same employee name will be aggregated.
HAVING COUNT(*) > 1  -->Filters the grouped results, retaining only those names that appear more than once.

```sql
SELECT ename,COUNT(*)
FROM emp
GROUP BY ename
HAVING COUNT(*)> 1;


/* arrange the data based on incresing order frequency*/
SELECT ename,COUNT(*)
FROM emp
GROUP BY ename
ORDER BY COUNT(*);

```





