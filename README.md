# Question 1

See **question1.html** for R implementation in RMarkdown

Extra - **question1.pynb** for python implementation sans explanation and plots

# Question 2

## a. How many orders were shipped by Speedy Express in total?

```
SELECT COUNT(OrderID) FROM Orders
WHERE ShipperID IN 
(
	SELECT ShipperID FROM Shippers 
	WHERE ShipperName='Speedy Express'
)
```

#### Result - 54


## b. What is the last name of the employee with the most orders?

```
SELECT LastName FROM Employees 
WHERE EmployeeID IN 
(
	SELECT EmployeeID FROM Orders
	GROUP BY EmployeeID
	ORDER BY COUNT(OrderID) DESC  
	LIMIT 1 -- Ordered the Number of Orders by Descending and only chose the first
)
```

#### Result - "Peacock"

## c. What product was ordered the most by customers in Germany?

```
SELECT ProductName FROM Products -- Product Names of Products of Orders of Customers from Germany
WHERE ProductID IN (
	SELECT ProductID FROM OrderDetails -- Products of Orders of Customers From Germany (Max Quantity)
	WHERE OrderID IN(
		SELECT OrderID FROM Orders -- Orders from customers from Germany
		WHERE CustomerID IN (
			SELECT CustomerID FROM Customers 
			WHERE Country = 'Germany')) -- Customers from Germany
	GROUP BY ProductID
	ORDER BY SUM(Quantity) DESC 
	LIMIT 1
)
```
#### Result - Boston Crab Meat

