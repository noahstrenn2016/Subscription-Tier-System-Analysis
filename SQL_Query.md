```sql

--Selects each customer name with their total sales and groups them into a tier list for a new 
--subscription based system.
select distinct a.customername,
				sum(b.amount) as total_sales,
	--Case statement for the new membership tier system based off total sales per customername 
	--Platinum level (more than 5000 total sales)
	--Gold level (between 2500 and 4999 total sales)
	--Silver level (between 500 and 24999 total sales)
	--Standard level (500 and less total sales)
				(case 
					when sum(b.amount) >= 5000 then 'Platinum'
					when sum(b.amount) >= 2500 and sum(b.amount) <= 4999 then 'Gold'
					when sum(b.amount) >= 500 and sum(b.amount) <= 2499 then 'Silver'
					else 'Standard'
				end) as membership_level
from list_of_orders a
--joining of tables 'list_of_orders' and 'order_details'
	inner join order_details b
	on a.order_id = b.order_id --order_id in both tables are matching
group by a.customername
order by sum(b.amount) desc
```
