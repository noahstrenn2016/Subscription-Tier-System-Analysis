This SQL query pulls some descriptive statistics that may be needed in our analysis. It will use a subqeury to pull the number of customers, average orders per customer, and average sales per customer within each membership_level group.


```sql
select 
	membership_level,
	count(customername) as num_of_customers,
	AVG(num_of_orders) as avg_orders_per_customer,
	AVG(total_sales) as avg_sales_per_customer
from
	(select distinct a.customername,
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
	count(distinct(a.order_id)) desc)
group by 
	membership_level
order by 
	avg_orders_per_customer desc;

```
