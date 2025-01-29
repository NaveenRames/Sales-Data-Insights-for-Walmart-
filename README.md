# Sales-Data-Insights-for-Walmart

SELECT * FROM saledatawalamrt.sales;

select 
time,
(case 
when time between "00:00:00" and "12:00:00" then "morning"
when time between "12:01:00" and "16:00:00" then "afternoon"
else "evening"
end) as time_of_date
from sales;

alter table sales add column time_of_day varchar(20);

update sales
set time_of_day=(
case 
when time between "00:00:00" and "12:00:00" then "morning"
when time between "12:01:00" and "16:00:00" then "afternoon"
else "evening"
end
);

SET SQL_SAFE_UPDATES = 0;

select 
date.
dayname(date) as day_name
from sales;


alter table sales add column day_name varchar(10);

update sales
set day_name =(dayname(date)
);


select 
date,
monthname(date)
from sales;


alter table sales add column month_name varchar(10);

update sales
set month_name =monthname(date);

-- Generic

-- how many unique cities does the data have?

select distinct city
from sales;

-- how many unique braches does the data have?

select 
distinct branch
from sales;

-- In which city each branch
select
distinct city,
branch
from sales;


-- -----------------------------Product-----------------------

-- How many unique product lines does the data have?

select 
count(distinct product_line)
from sales;

-- what is the most common payment method?

select 
payment_method,
count(payment_method) as cnt
from sales
group by payment_method
order by cnt desc;

-- what is the most selling product line?
select 
product_line,
count(*) as pl
from
sales
group by product_line
order by pl desc;


-- what is the total revenue by month?
select
month_name as month,
sum(total) as total_revenue
from sales
group by month_name
order by total_revenue desc;

-- what month had the largest COGS?
select
month_name as month,
sum(cogs) as cogs
from sales
group by month_name
order by cogs desc;

-- what product line had the largest revenue?
select
product_line,
sum(total) as total_revenue
from sales
group by product_line
order by total_revenue desc;


-- what is the city with largest revenue?

select 
branch,
city,
sum(total) as total_revenue
from sales
group by city, branch
order by total_revenue desc;

-- what product line had the largest VAT?
select 
product_line,
avg(VAT) as avg_tax
from sales
group by product_line
order by avg_tax desc;

-- which branch sold more products than average product sold?

select 
branch,
sum(quantity) as qty
from sales
group by branch
having sum(quantity) > (select avg(quantity) from sales);

-- what is the most common product line by gender?

select 
gender,
product_line,
count(gender) as total_cnt
from sales
group by gender, product_line
order by total_cnt desc; 

-- what is the average rating of each product line?

select 
round(avg(rating),2) as avg_rating,
product_line
from sales
group by product_line
order by avg_rating desc;
