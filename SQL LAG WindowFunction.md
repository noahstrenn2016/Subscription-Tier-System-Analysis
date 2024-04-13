This SQL query pulls each customer name with their average number of days between their orders.


```sql
SELECT Customername, 
	truncate((AVG(Order_Date - PriorDate)),0) as avg_days_between_orders
FROM 
	(SELECT
		Customername,
		Order_Date,
		LAG(Order_Date) OVER (PARTITION BY Customername ORDER BY Order_Date) as PriorDate
FROM 
	list_of_orders)
GROUP BY 
	Customername;
```
