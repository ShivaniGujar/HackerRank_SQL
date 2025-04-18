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
# 5. Pattern Matching in SQL (Basics) LIKE Operator

**A. Display the employee name whose name starts with 'M'**

  % reprsent rest caan be anything

```sql
SELECT ename FROM emp
WHERE ename LIKE 'M%';
```


**B. Display the employee name whose name ends with 'N'**

  % reprsent rest caan be anything

```sql
SELECT ename FROM emp
WHERE ename LIKE '%N';
```

**C. Display the employee name of all employee having 'M' in any position in there name**

  diplay of emplyee whose have M in there name

```sql
SELECT ename FROM emp
WHERE ename LIKE '%M%';
```


**D. Display the employee name of all employee does not contain 'M' in any position in there name**

 

```sql
SELECT ename FROM emp
WHERE ename NOT LIKE '%M%';
```


# 6. Pattern Matching in SQL (Basics) LIKE Operator

**1.Display the names of all employee whose name contain exactly 4 letters**

  LIKE '____': The underscore (_) represents one character, and since we use four underscores, 
  it ensures that only names with exactly 4 characters are selected

```sql
SELECT ename
FROM emp
WHERE ename LIKE '____';

/*Query Using LENGTH(): LENGTH(ename) = 4: This checks if the number of characters in ename is exactly 4.*/

SELECT ename 
FROM emp 
WHERE LENGTH(ename) = 4;



```

**2.Display the names of  employee whose name contain the**
**(i) second letter as 'L'**

_ (underscore) represents any single character.
L is the second letter.
% (percent) represents any sequence of characters after L.

```sql
SELECT ename
FROM emp
WHERE ename LIKE '_L%';
```


**(ii) Fourth letter as 'M'**


```sql
SELECT ename
FROM emp
WHERE ename LIKE '___M%';
```

**3. Display the employee names and hire dates for the employee who joined the month of december**

```sql
SELECT ename,hiredate
FROM emp
WHERE hiredate LIKE '%DEC%';
```

**4. Display the employee names whose name contains exactly 1 'L's**

Involve 2 LL , starting and ending can be anything

```sql
SELECT ename
FROM emp
WHERE ename LIKE '%LL%';
```

**5. Display the employee names whose name starts with 'J' and ends with 'S'**

```sql
SELECT ename
FROM emp
WHERE ename LIKE 'J%S';
```

# 7. Display nth row in sql 


Might ask to find 2nd row in table ,or 10th row in table

***ORACLE***
ROWNUM is not work greater than > and is equal to (= ) operator
if we want to display 4th record  using rownum we use minus 
```sql

/*Oracle use ROWNUM :
SELECT * FROM emp WHERE ROWNUM <= 4: Retrieves the first 4 rows from the emp table.
SELECT * FROM emp WHERE ROWNUM <= 3: Retrieves the first 3 rows from the emp table.
MINUS: Subtracts the second query's result from the first, leaving only the 4th row
 */

SELECT * FROM emp
WHERE ROWNUM <=4
MINUS
SELECT * FROM emp
WHERE ROWNUM <=3;

OR
/*alias create for rownum so we can work with = */
SELECT * FROM
(SELECT ROWNUM AS r , ename, sal FROM emp)
WHERE r=4;


/*if want to display all column*/
SELECT * FROM
(SELECT ROWNUM AS r,emp.* FROM emp)
WHERE r=4;

/*if want to display record in 2nd ,3th and 7th records i.e display any random records */
SELECT * FROM
(SELECT ROWNUM AS r,emp.* FROM emp)
WHERE r in(2,3,7);

```

***MYSQL***

```sql

/*OFFSET 3 skips the first 3 rows.
LIMIT 1 fetches the 4th row.*/

SELECT ename, sal
FROM emp
ORDER BY sal
LIMIT 1 OFFSET 3;
```


# 8. UNION vs UNIONALL

**UNION**

Removes duplicate rows from the final result.
Performs sorting internally to eliminate duplicates, making it slower compared to UNION ALL.
Useful when you need a unique list of records.

    ex : SELECT ename, sal FROM emp1
    UNION
    SELECT ename, sal FROM emp2;

 **UNIONALL**

Does not remove duplicates; keeps all records.
Faster than UNION because it does not sort or filter duplicates.
Useful when you need all data, including duplicates
ex: 
        SELECT ename, sal FROM emp1
        UNION ALL
        SELECT ename, sal FROM emp2;

# 9. JOINS 

What is a JOIN?
A JOIN in SQL is used to combine rows from two or more tables based on a related column between them
(usually a foreign key).

Types of SQL JOINs:
INNER JOIN -> Only rows with matching keys in both tables
LEFT JOIN -> All rows from the left table + matching from the right
RIGHT JOIN -> All rows from the right table + matching from the left
FULL JOIN -> All rows from both tables, matched where possible

INNER JOIN
- Returns only matching rows from both tables.
- If no match, the row is excluded.
- JOIN and INNER JOIN are the same

```sql
Syntax:
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.common_column = table2.common_column;

SELECT ProductID, ProductName, CategoryName
FROM Products
INNER JOIN Categories
ON Products.CategoryID = Categories.CategoryID;

/*Example 1: Employees in Chicago*/

SELECT ename, sal, d.deptno, dname, loc
FROM emp AS e, dept AS d
WHERE e.deptno = d.deptno AND loc = 'Chicago';

/*Example 2: Department-wise Total Salaries*/

SELECT dname, SUM(sal)
FROM emp AS e, dept AS d
WHERE e.deptno = d.deptno
GROUP BY d.dname;

/*Joining 3 Tables*/

SELECT Orders.OrderID, Customers.CustomerName, Shippers.ShipperName
FROM ((Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID)
INNER JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID);
```
# 10. SELF JOIN

Joining a table with itself is called self join
A self join is a regular join, but the table is joined with itself.
Compared values with the values of same column of itself or different values with same table
we need to create replica for same column as ALias

```sql
Syntax:
SELECT column_name(s)
FROM table1 T1, table1 T2
WHERE condition;
```

**Display emplyee details who are getting more salary than theie manager salary**
here have only one salary column
**Display emplyee details who joined before there manageer**
in here have only one date column


