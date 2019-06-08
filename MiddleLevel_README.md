## 1.[Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/)
#### Solution 1
```
SELECT customer_id
FROM (
    SELECT customer_id , COUNT(DISTINCT product_key) AS num_orders
    FROM Customer
    GROUP BY customer_id ) tmp
WHERE num_orders= (SELECT COUNT(DISTINCT product_key) FROM Product);
```
#### Solution 2
```
SELECT customer_id
FROM Customer 
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (
SELECT COUNT(DISTINCT product_key)
FROM Product );
```

Notes:
1. Don't forget **DISTINCT product_key**. The table only has two values 5,6, however, we should get rid of possible duplicate values in product table. 

## 2.[Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/)
```
SELECT Name
FROM Employee
WHERE Id In (
    SELECT ManagerId 
    FROM Employee 
    GROUP BY ManagerId
    HAVING COUNT(*)>=5);   
```

## 3. [Rank Scores](https://leetcode.com/problems/rank-scores/)
#### MySQL Server Solution
```
SELECT Score, DENSE_RANK()OVER (ORDER BY Score DESC) AS Rank
FROM Scores;
```

## 4.[Investments in 2016](https://leetcode.com/problems/investments-in-2016/)
```
SELECT SUM(TIV_2016) AS TIV_2016
FROM insurance
WHERE (LAT,LON) IN (
    SELECT DISTINCT LAT,LON
    FROM insurance 
    GROUP BY LAT,LON
    HAVING COUNT(*)=1)
AND
TIV_2015 IN (SELECT TIV_2015             
            FROM insurance
            GROUP BY TIV_2015
            HAVING COUNT(*)>1);
```            
Notes:
1.**SUM()** instead of sum () [without space]
2.Should not include the unique value of TIV_2015. It means that same value occurs multiple times (using count()>1). I always forgot this part!!
3. **DISTINCT X,Y** instead of DISTINCT(X,Y)[without bracket]
4. Should use () in WHERE if there are multiple columns included.
5. More practice!

## 5.[Winning Candidate](https://leetcode.com/problems/winning-candidate/)
``` 
SELECT Name
FROM Candidate
WHERE id = (
SELECT CandidateId
FROM Vote
GROUP BY CandidateId
ORDER BY COUNT(DISTINCT id) DESC
Limit 1);
``` 
Notes:
1. Builds connect between id & CandidateId

## 6.[Second Degree Follower](https://leetcode.com/problems/second-degree-follower/)
``` 
SELECT followee as follower,COUNT(*) AS num
FROM follow
WHERE followee IN
    (SELECT DISTINCT follower
     FROM follow
     GROUP BY 1) 
GROUP BY 1
ORDER BY followee;
``` 
## 7.[Tree Node](https://leetcode.com/problems/tree-node/)
```
SELECT id, 'Root' AS Type
FROM tree
WHERE p_id IS NULL
UNION 
SELECT id, 'Inner' AS Type
FROM tree
WHERE id IN (
    SELECT p_id 
    FROM tree
    GROUP BY p_id
    HAVING p_id IS NOT NULL)
AND p_id IS NOT NULL
UNION 
SELECT id, 'Leaf' AS Type
FROM tree
WHERE id NOT IN (
    SELECT p_id
    FROM tree
    GROUP BY p_id
    HAVING p_id IS NOT NULL)
AND p_id IS NOT NULL;
```
Notes:
1. Always pay more attention to NULL. For example, We need include **HAVING p_id IS NOT NULL** to only get numbers 
2. HAVING is kind of similar to WHERE. So We can use HAVING with condition which is not limited to COUNT().

## 8.[Shortest Distance in a Plane](https://leetcode.com/problems/shortest-distance-in-a-plane/)
```
SELECT ROUND (SQRT(MIN(POW(p1.x- p2.x, 2) + POW(p1.y-p2.y,2))),2) AS shortest
FROM point_2d p1
JOIN point_2d p2
ON p1.x != p2.x OR p1.y != p2.y
```
Notes:
1. [POW(x, y) x:the base y:the exponent](https://www.w3schools.com/sql/func_mysql_pow.asp)
2. ?? Concern about why did I use **OR** (returns 1.4 when I used **AND**)

## 9.[Friend Requests II: Who Has the Most Friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/)
```
SELECT ids AS id, COUNT(ids) AS num
FROM(
    SELECT requester_id AS ids
    FROM request_accepted 

UNION ALL
    SELECT accepter_id AS ids
    FROM request_accepted ) a
GROUP BY ids
ORDER BY num DESC
Limit 1;
```
Notes:
1. **UNION ALL** will return ["ids"],"values":[[1],[1],[2],[3],[2],[3],[3],[4]] one column by one column.It looks like the "append" in python.
2. Generate know column name ids which contains all values after "append"
3. Don't forget to rename the column name. ids-->id


## 10.[Count Student Number in Departments](https://leetcode.com/problems/count-student-number-in-departments/)
```
SELECT d.dept_name, IFNULL(COUNT(s.student_id),0) AS student_number 
FROM student s
RIGHT JOIN department d
ON d.dept_id = s.dept_id
GROUP BY d.dept_id
ORDER BY student_number DESC, dept_name ;
```
Notes:
1. IFNULL(,0) 
2. student, **department** --> RIGHT JOIN;  **department**, student---> LEFT JOIN
3. Because we are focusing on the table department, so we should using d.dept_id in GROUP BY part ( s.dept_id dose not work)
4. Cares for the ORDER BY for multiple columns

## 11.[Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/)
```
SELECT product_id, year AS first_year, quantity, price
FROM Sales
WHERE (product_id,year) IN (
SELECT product_id, min(year) as year
FROM Sales
GROUP BY product_id); 
```
Notes:
1. USING GROUP BY two columns 

## 12.[Winning Candidate](https://leetcode.com/problems/winning-candidate/)
```
SELECT Name
FROM Candidate
WHERE id = (
    SELECT CandidateId 
    FROM Vote
    GROUP BY CandidateId
    ORDER BY COUNT(*) DESC
    LIMIT 1);
 ```
 Notes:
 1. Should be wise to select id =() or id in (). Because there is only one record will be returned, so it should be used "="
 
 
 ## 13.[Get Highest Answer Rate Question](https://leetcode.com/problems/get-highest-answer-rate-question/)
 #### Solution 1
 ```
SELECT question_id AS survey_log
FROM survey_log
GROUP BY question_id
ORDER BY COUNT(answer_id)/COUNT(IF(action= 'show',1,0)) DESC 
LIMIT 1;
```
Notes:
1. The most important thing is to understand the question. Caculate the rate: count(answer)/ count(show)--> order by the value DESC. COUNT(answer)<--- COUNT(answer_id);  count(show)<--- COUNT (action) with condition by setting action='show'. It is hard to differentiate the show and skip by using column answer_id because both of their values are NULL. It is better to select column action here.

 #### Solution 2
 ```
SELECT question_id AS survey_log
FROM survey_log
GROUP BY question_id
ORDER BY (SUM(action="answer")/SUM(action="show")) DESC 
LIMIT 1;
 ```
Notes:
1. Only focus on the column action using **SUM**
2. ORDER BY sum(action= 'answer')/COUNT(IF(action= 'show',1,0)) DESC  could work as well. However, it would not work using 
COUNT(IF(action= 'answer',1,0)).I don't know why??

## 14.[Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers/)
```
SELECT DISTINCT l1.Num AS ConsecutiveNums 
FROM Logs l1, Logs l2, logs l3
WHERE   l1.Id =l2.Id-1 
    AND l2.Id = l3.Id-1 
    AND l1.Num =l2.Num 
    AND l2.Num =l3.Num
```
```
SELECT DISTINCT l1.Num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2
JOIN logs l3
ON l1.Id = l2.Id-1 AND l2.Id = l3.Id-1
WHERE l1.Num = l2.Num AND l2.Num =l3.Num
```
Notes:
1. The usual way is to select 3 tables (depends on how many consecutive numbers should have)
2. **Distinct**
3. **l1.Id = l2.Id-1 AND l2.Id = l3.Id-1** At first I used the l1.Id !=l2.Id AND l2.Id != l3.Id which could not control the number comes from the place just right before and right.


## 15. [Department Highest Salary](https://leetcode.com/problems/department-highest-salary/)
```
SELECT d.Name AS Department, e.Name AS Employee, e.Salary
FROM Employee e 
JOIN Department d
ON d.Id =  e.DepartmentId
WHERE (e.DepartmentId, e.Salary) IN
    (SELECT DepartmentId, MAX(Salary)
     FROM Employee
     GROUP BY DepartmentId); 
```
Notes:
1. Need more practice
2. Buddle (DepartmentId,Salary)

## 16.[Second Degree Follower](https://leetcode.com/problems/second-degree-follower/)

```
SELECT a.follower ,COUNT(DISTINCT b.follower) AS num
FROM follow a
JOIN follow b 
ON a.follower = b.followee
GROUP BY a.follower
ORDER BY a.follower;
```
Notes:
1. Need more practice!
2. COUNT(DISTINCT **b**.follower) 


## ??.[Exchange Seats](https://leetcode.com/problems/exchange-seats/)
```
SELECT id, iif(id%2!=0, lead(student,1,student) over (ORDER BY id), lag(student,1) over (ORDER BY id)) as student FROM seat;
```

