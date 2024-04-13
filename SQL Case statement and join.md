This SQL query returns each customer name with their total spending and number of purchases and groups them into a tier list for a new subscription-based tier system. Uses a case statement to create the new membership_level field and uses a join to join the two table together based off of the order_id.
```sql
select distinct a.customername,
		sum(b.amount) as total_sales, 
		count(distinct(a.order_id)) as num_of_orders,
		(case 
			when sum(b.amount) >= 5000 then 'Platinum'
			when sum(b.amount) >= 2500 and sum(b.amount) <= 4999 then 'Gold'
			when sum(b.amount) >= 500 and sum(b.amount) <= 2499 then 'Silver'
			else 'Standard'
		end) as membership_level
from 
	list_of_orders a
	inner join order_details b on a.order_id = b.order_id 
group by 
	a.customername
order by 
	count(distinct(a.order_id)) desc;
```
