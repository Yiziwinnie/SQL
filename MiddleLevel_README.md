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

## ??.[Exchange Seats](https://leetcode.com/problems/exchange-seats/)
???
