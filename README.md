# Customer_Shopping_Behaviour_Analysis

# Project Overview
This project analyzes customer shopping behaviour using Power BI to identify
sales patterns, customer preferences, and key business insights across
season, gender, product size, and location.

# Objective
The objective of this project is to understand customer purchasing behaviour
and help businesses make data-driven decisions to improve sales and customer
engagement.

# Tools and Technologies used
- Excel / CSV Dataset
- SQL Server for Data Extraction and Analysis
- Python for Data Cleaning & Transformation
- Power Bi for Data Visualization

# Dashboard Overview
<img width="866" height="591" alt="image" src="https://github.com/user-attachments/assets/ac007161-299a-446a-89d1-c8635ff30b1b" />
<img width="950" height="542" alt="image" src="https://github.com/user-attachments/assets/f717bc40-8f2a-4913-a0f4-2b196ea94af3" />

# SQL Analysis
Below are some SQL Queries used during data analysis

# Top 3 most purchased products within each category 

with item_count as 
( 
select category,item_purchased,count(customer_id) as total_orders,
row_number() over (partition by category order by count(customer_id) desc) as item_rank
from sales_table
group by category,item_purchased
)
select item_rank,category,item_purchased,total_orders
from item_count
where item_rank <= 3

# Do subscribed customers spend more ? Compare average spend and total revenue between subscribers and non-subscribers

select subscription_status,
count(customer_id) as total_customers,
avg(purchase_amount) as avg_amount,
sum(purchase_amount) as total_revenue from sales_table
group by subscription_status

# segment customers into new,returning and loyal based on their total no of previous purchases and show the count of each segment 

with customer_seg as 
(
select customer_id,previous_purchases,
case 
   when previous_purchases = 1 then 'new'
   when previous_purchases between 2 and 10 then 'returnig'
   else 'loyal' end as customer_type
   from sales_table
)
select customer_type,count(*) as "no_of_customers" from customer_seg
group by customer_type
