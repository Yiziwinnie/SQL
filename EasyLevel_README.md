## 1.[Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/)
```
SELECT p.product_name, s.year, s.price
FROM Sales s
JOIN Product p
on s.product_id = p.product_id ;
```

## 2.[Product Sales Analysis II](https://leetcode.com/problems/product-sales-analysis-ii/)
```
SELECT s.product_id, SUM(s.quantity) AS total_quantity 
FROM Sales s
JOIN Product p
on s.product_id = p.product_id 
GROUP BY s.product_id
ORDER BY total_quantity DESC;
```

## 3.[Actors and Directors Who Cooperated At Least Three Times](https://leetcode.com/problems/actors-and-directors-who-cooperated-at-least-three-times/)
```
SELECT actor_id, director_id 
FROM (
SELECT actor_id, director_id, COUNT(*)
FROM ActorDirector
GROUP BY actor_id, director_id
HAVING COUNT(*)>=3 ) A ;
```

## 4.[Big Countries](https://leetcode.com/problems/big-countries/)
```
SELECT name, population, area
FROM World
WHERE population > 25000000 OR area > 3000000
ORDER BY name ASC;
```
Note: Should be pay more attention to the number

## 5.[Shortest Distance in a Line](https://leetcode.com/problems/shortest-distance-in-a-line/)
```
SELECT min(DISTINCT abs(p1.x- p2.x)) AS shortest
FROM point p1
JOIN point p2
on p1.x != p2.x;  
```
Notes:
1. The point is only one cordinate, so it only needs to do subtraction. p1.x-p2.x <br/>
2. abs()
3. Distinct() It can get rid of duplicated minimum distance.
4. Min()
5. The order of distinct() and min() could be swamped.<br/>
   For example, ```SELECT DISTINCT min(abs(p1.x- p2.x)) AS shortest```

## 6.[Swap Salary](https://leetcode.com/problems/swap-salary/)
```
UPDATE salary
SET sex= CASE sex
    WHEN 'f' THEN 'm'
    ELSE'f'
    END; 
```
Notes:
1. Only allows to use UPDATE 
2. UPDATE [Table] --SET[Column]--[Column]= CASE Column  WHEN....THEN...ELSE...END;

## 7.[Find Customer Referee](https://leetcode.com/problems/find-customer-referee/)
#### Solution 1
```
SELECT name 
FROM customer
WHERE referee_id != 2 OR referee_id IS NULL;
```
#### Solution 2
```
SELECT name 
FROM customer 
WHERE id NOT IN (
SELECT id 
FROM customer 
WHERE referee_id = 2);
```
Notes
1. Always remember NULL and double check with it. referee_id != 2 only returns number which exludes NULL. 
2. Find a bridge **id**    **id** not in ( select **id** .....)

## 8.[Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)
```
SELECT id, movie, description, rating 
FROM cinema
WHERE description NOT LIKE "%boring%" AND id%2 != 0 
ORDER BY rating DESC;
```

## 9.[Triangle Judgement](https://leetcode.com/problems/triangle-judgement/)
```
SELECT x,y,z,
CASE 
    WHEN x+y >z AND x+z>y AND y+z >x THEN 'Yes'
    ELSE'No'
    END AS triangle
FROM triangle;
```

```
SELECT *,
CASE 
    WHEN x+y>z AND x+z>y AND y+z>x 
    THEN 'Yes'
    ELSE 'No'
    END AS triangle
FROM triangle;
```

Notes:
1. Create a new column with conditions using CASE...WHEN (...THEN..)...ELSE() END AS **[Column Name]**
2. Don't forget the , before CASE

## 10.[Employee Bonus](https://leetcode.com/problems/employee-bonus/)
```
SELECT name,bonus 
FROM Employee e
LEFT JOIN Bonus b
ON e.empId = b.empId
WHERE b.bonus <1000 OR b.bonus IS NULL;
```
Notes
1. Always remember NULL and double check with it. b.bonus <1000 only returns number which exludes NULL. 

## 11.[Consecutive Available Seats](https://leetcode.com/problems/consecutive-available-seats/)
```    
SELECT DISTINCT c1.seat_id
FROM cinema c1
JOIN cinema c2
on c1.seat_id != c2.seat_id
WHERE abs(c1.seat_id -c2.seat_id) =1 AND c1.free = 1 AND c2.free= 1
ORDER BY c1.seat_id ASC;
```
Notes:
1.distinct c1.seat_id<br/>
2.abs()=1 AND c1.free= 1 AND c2.free =1<br/>
3.ORDER BY<br/>
4.The consecutive number refers to 2 consecutive numbers. There are only two tables joined.<br/>

## 12.[Sales Person](https://leetcode.com/problems/sales-person/)
#### Solution 1
```    
SELECT name 
FROM salesperson 
WHERE sales_id NOT IN (
SELECT sales_id FROM orders WHERE com_id=(
SELECT com_id FROM company WHERE name LIKE "%RED"));
```    
#### Solution 2
```  
SELECT name 
FROM salesperson 
WHERE sales_id NOT IN (
SELECT DISTINCT o.sales_id 
FROM orders o
JOIN company c
ON c.com_id = o.com_id
WHERE c.name LIKE "%RED%");
```  

## 13.[Duplicate Emails](https://leetcode.com/problems/duplicate-emails/)
```  
SELECT Email 
FROM Person
GROUP BY Email 
HAVING COUNT(Email)>1;
```  
## 14.[Combine Two Tables](https://leetcode.com/problems/combine-two-tables/)
```  
SELECT FirstName, Lastname, City, State 
FROM Person p
LEFT JOIN Address a
ON p.PersonId = a.PersonId ;
```  
Note:
1. Should use **LEFT JOIN** because people may have name but may not have addresses. 

## 15.[Employees Earning More Than Their Managers](https://leetcode.com/problems/employees-earning-more-than-their-managers/)
#### Solution 1
```  
SELECT e1.Name AS Employee
FROM Employee e1
JOIN Employee e2
ON e1.ManagerId = e2.Id
WHERE e1.Salary > e2.Salary;
```  
#### Solution 2
```  
SELECT e1.Name as Employee
FROM Employee e1, Employee e2
WHERE e1.ManagerId = e2.Id AND e1.Salary > e2.Salary ;
```  
Notes:
1. Looks like append new columns behind of orgirnal columns **e1.ManagerId = e2.Id**

## 16.[Customers Who Never Order](https://leetcode.com/problems/customers-who-never-order/)
#### Solution 1
```
SELECT Name As Customers
FROM Customers
WHERE Id NOT IN (
SELECT CustomerId 
FROM Orders );
```
#### Solution 2
```
SELECT c.Name AS Customers
FROM Customers c
LEFT JOIN Orders o
ON c.Id =o.CustomerId
WHERE O.Id IS NULL;
```
Notes:
1. Should use the **LEFT JOIN**

## 17.[Biggest Single Number](https://leetcode.com/problems/biggest-single-number/)
```
SELECT MAX(num) AS num
FROM (
SELECT DISTINCT num  
FROM my_numbers
GROUP BY num 
HAVING COUNT(num)=1) A ;
```

## 18.[Classes More Than 5 Students](https://leetcode.com/problems/classes-more-than-5-students/)
```
SELECT DISTINCT(class)
FROM courses
GROUP BY class
HAVING COUNT(DISTINCT(student))>=5;
```
Notes:
1. Should include **DISTINCT** 

## 19.[Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/)
```
DELETE p1 
FROM Person p1, Person p2
WHERE p1.Email =p2.Email AND p1.Id >p2.Id;
```
Notes:
1. Practice more to get this logic
2. The duplicated numbers start from larger number, so we need to remove them
3. Only allows to use **DELETE**

## 20.[Second Highest Salary](https://leetcode.com/problems/second-highest-salary/)
#### Solution 1
```
SELECT IFNULL((SELECT DISTINCT Salary
             FROM Employee
             ORDER BY Salary DESC
             LIMIT 1 OFFSET 1),NULL) AS SecondHighestSalary ;
```

#### Solution 2
```
SELECT Max(Salary) AS SecondHighestSalary
FROM Employee
WHERE Salary NOT IN 
(SELECT Max(Salary)
FROM Employee);
```

Notes:
1. However, this solution will be judged as 'Wrong Answer' if there is no such second highest salary since there might be only one record in this table. To overcome this issue, we can take this as a temp table.<br/>
2. Practice more **OFFSET 1** & **IFNULL**
3. **DISTINCT**


## 21.[Rising Temperature](https://leetcode.com/problems/rising-temperature/)
```
SELECT w2.Id 
FROM Weather w1, Weather w2
WHERE datediff(w2.RecordDate,w1.RecordDate) =1 AND w1.Temperature < w2.Temperature;
```
Notes:
1. Pay attention to the order of w1, w2
2. **datediff (,)** using","instead of using "-" & lower case of datediff(,)

## 22.[Friend Requests I: Overall Acceptance Rate](https://leetcode.com/problems/friend-requests-i-overall-acceptance-rate/)
#### My SQL Solution 
```
SELECT ROUND(
IFNULL(
(SELECT COUNT(DISTINCT requester_id, accepter_id) FROM RequestAccepted) /
(SELECT COUNT(DISTINCT sender_id, send_to_id) FROM FriendRequest )
,0)
,2) AS accept_rate
```


Notes:
1. round & IFNULL: round **(** IFNULL([],0),2 **)** AS accept_rate
2. DISTINCT!! 
3. SELECT COUNT() FROM **(** SELECT DISTINCT xx,xx FROM YY GROUP BY xx,xx **)** AS **Z**
4. More practice !!


## 23.[Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/)
```
SELECT product_id, year AS first_year, quantity, price
FROM Sales
WHERE (product_id, year) IN (
SELECT product_id, MIN(year) as year
FROM Sales
GROUP BY product_id) ;
```
Notes:
1. Boundle product_id, min(year)

## 24.[Customer Placing the Largest Number of Orders](https://leetcode.com/problems/customer-placing-the-largest-number-of-orders/)
```
SELECT customer_number
FROM(
SELECT customer_number,COUNT(*) AS order_number
FROM orders
GROUP BY customer_number
ORDER BY order_number DESC) A 
LIMIT 1;
```
#### Easy solution
```
SELECT customer_number
FROM orders
GROUP BY customer_number
ORDER BY COUNT(*) DESC
LIMIT 1;
```
