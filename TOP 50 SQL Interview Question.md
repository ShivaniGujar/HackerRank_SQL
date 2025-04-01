# 1.  Find Second Highest Salary of an Employee
Explanation:

***Approach 1: Subquery Approch with NOT IN***

Here first we are creating resultset  excluding first highest salary using i.e max(salary),
again applying max(salary) on this result would fetch second highest salary

```sql
SELECT max(sal) FROM employee
WHERE sal NOT IN (SELECT max(sal) FROM employee);
 ```

***Approach 2 : Subquery with < operator***

```sql
   SELECT max(sal) FROM emp
   WHERE sal <(SELECT max(sal) FROM employee);
```


