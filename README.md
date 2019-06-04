
## 1.[Product Sales Analysis II](https://leetcode.com/problems/product-sales-analysis-ii/)

## MySQL Solution
```
SELECT s.product_id, SUM(s.quantity) AS total_quantity 
FROM Sales s
JOIN Product p
on s.product_id = p.product_id 
GROUP BY s.product_id
ORDER BY total_quantity DESC;
```

## 2.[Actors and Directors Who Cooperated At Least Three Times](https://leetcode.com/problems/actors-and-directors-who-cooperated-at-least-three-times/)
## My SQL Solution
```
SELECT actor_id, director_id 
FROM (
SELECT actor_id, director_id, COUNT(*)
FROM ActorDirector
GROUP BY actor_id, director_id
HAVING COUNT(*)>=3 ) A ;
```
