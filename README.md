# Retail Sales Analysis SQL Project

## Project Overview

**Project Name**: SQL Retail Sales Analysis - P1 
**Level**: Beginner  
**Database**: `sql_project_p1`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure ##

### Database Setup

- **Database Creation**: The project starts by creating a database named `sql_project_p1`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

-- Create Database
create database sql_project_p1
-- Create table
DROP TABLE IF EXISTS retail_sales;
Create table retail_sales
(
	transactions_id INT PRIMARY KEY,
	sale_date DATE,
	sale_time TIME,	
	customer_id	INT,
	gender VARCHAR(25),	
	age	INT,
	category VARCHAR(50),	
	quantiy INT,	
	price_per_unit FLOAT,	
	cogs FLOAT,	
	total_sale FLOAT
);

---> Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

-- How many sales do we have?
select 
	count(*) as total_sale 
from retail_sales;

-- How many unique customers do we have?
select 
	count(distinct customer_id) 
from retail_sales;

-- What are our unique categories?
select 
	distinct category 
from retail_sales;

-- Checking for null values or missing data
select * from retail_sales
where 
	transactions_id is null OR sale_date is null OR sale_time is null
	OR customer_id is null OR gender is null OR age is null
	OR category is null OR quantiy is null OR price_per_unit is null
	OR cogs is null OR total_sale is null;

DELETE FROM retail_sales
where
	transactions_id is null OR sale_date is null OR sale_time is null
	OR customer_id is null OR gender is null OR age is null
	OR category is null OR quantiy is null OR price_per_unit is null
	OR cogs is null OR total_sale is null;


---> Data Analysis: Business Key Problems & Answers

The following SQL queries were developed to answer specific business questions.

-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05'.
select * 
from retail_sales
where 
	sale_date = '2022-11-05';

-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more thean 4 in the month of Nov-2022.
select * 
from retail_sales
where 
	category = 'Clothing'
	and
	TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
	and
	quantiy >= 4;

-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
select 
	category, 
	sum(total_sale) as net_sale, 
	count(*) as total_orders
from retail_sales
group by category;

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
select 
	category, 
	round(avg(age), 2) as average_age
from retail_sales
where
	category = 'Beauty'
group by category;

-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
select * 
from retail_sales
where
	total_sale > 1000;

-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
select 
	category, 
	gender, 
	count(*) as total_transactions
from retail_sales
group by category, gender
order by 1;

-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year. -->> Important Question.
select * 
from
(
select
	extract(year from sale_date) as year,
	extract(month from sale_date) as month,
	avg(total_sale) as average_sale,
	rank() over(partition by extract(year from sale_date) order by avg(total_sale) desc) as ranks
from retail_sales
group by 1, 2
) as t1
where ranks = 1;

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales.
select 
	customer_id, 
	sum(total_sale) as total_sales
from retail_sales
group by customer_id
order by total_sales desc limit 5;

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
select 
	category, 
	count(distinct customer_id) as cnt_unique_cs
from retail_sales
group by category;

-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17). -->> Important Question.
with hourly_sales
as
(
select *,
	case
		when extract(hour from sale_time) <12 then 'Morning'
		when extract(hour from sale_time) between 12 and 17 then 'Afternoon'
		else 'Eveining'
	end as shift
from retail_sales
)
select 
	shift, 
	count(*) as total_orders
from hourly_sales
group by shift
order by total_orders desc;

-- END OF PROJECT --

---> Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

---> Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

---> Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

---> User Guide

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

---> Author - Maedeh Dehghan

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

**E-mail Address**: [mdehghan9397@gmail.com]

Thank you for your support, and I look forward to connecting with you!
