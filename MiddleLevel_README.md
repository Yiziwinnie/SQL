## 1.[Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/)
```
SELECT customer_id
FROM (
    SELECT customer_id , COUNT(DISTINCT product_key) AS num_orders
    FROM Customer
    GROUP BY customer_id ) tmp
WHERE num_orders= (SELECT COUNT(DISTINCT product_key) FROM Product);
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

## 3.[Exchange Seats](https://leetcode.com/problems/exchange-seats/)
???

## 4. [Rank Scores](https://leetcode.com/problems/rank-scores/)
#### MySQL Server Solution
```
SELECT Score, DENSE_RANK()OVER (ORDER BY Score DESC) AS Rank
FROM Scores;
