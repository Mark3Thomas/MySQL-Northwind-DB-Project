# 🛒 Northwind MySQL Project

This project involves creating and analyzing the classic **Northwind database** using **MySQL**. The Northwind dataset represents a fictional trading company that imports and exports specialty foods worldwide. It includes details about customers, employees, orders, products, suppliers, and more—making it ideal for practicing relational database design and SQL queries.

---

## 📌 1. Project Overview

This repository demonstrates how to:
- Rebuild the **Northwind schema** using SQL DDL (Data Definition Language).
- Populate tables with sample business data.
- Query the dataset to extract insights on customer behavior, product performance, and supply chain activity.

---

## 🎯 2. Project Objectives

- Build a relational database using structured SQL commands.
- ER diagrams
- Explore multi-table relationships using **JOINs**, **aggregations**, and **filters**.
- Generate business insights through analytical SQL queries.
- Simulate real-world database interactions in a retail context.

---

## 🗂 3. Schema Overview

The database contains the following key tables:

| Table                | Description                                             |
|---------------------|---------------------------------------------------------|
| `customers`          | Customer info, contact, and location details           |
| `orders`             | Sales order data, linked to customers and employees    |
| `order_details`      | Products, prices, and quantities per order             |
| `products`           | Product names, categories, pricing, and stock info     |
| `suppliers`          | Supplier companies and their contact information       |
| `employees`          | Employee data including roles and managers             |
| `categories`         | Product categories (e.g., Beverages, Produce)          |
| `shippers`           | Companies responsible for shipping orders              |
| `regions`, `territories`, `employee_territories` | Employee and regional mapping |

📄 *All tables, keys, constraints, and indexes were created using the SQL script: `Northwind Database create.sql`.*
![image](https://github.com/user-attachments/assets/4435981f-0be7-4265-9537-6f8e21e86bdf)

---

## 📊 4. Dataset Overview

The Northwind dataset includes:

- ✔️ Dozens of products from 8 categories
- ✔️ 29 suppliers and 91 customers across different countries
- ✔️ 830+ customer orders
- ✔️ 9 employees and a hierarchy of reporting territories
- ✔️ Realistic sales transactions and shipping records

This sample dataset allows you to simulate queries found in retail, sales, logistics, and CRM systems.

---
## 🛠 5 Tools Used

| Tool                  | Purpose                                              |
|-----------------------|------------------------------------------------------|
| **MySQL**             | Database engine for executing queries                |
| **SQL Workbench / DBeaver** | SQL IDE for writing, testing, and managing scripts     |
| **Northwind create.sql** | SQL file to generate database schema and structure     |
| **Excel** *(optional)* | Used for previewing, exporting, and formatting results |
| **ERD Tool (optional)** | Visualizing relationships between database tables    |

## 🔍 6 Key Insights

- 📦 **Top 5 customers** account for a significant portion of overall revenue—highlighting strategic clients.
- 🗓️ Sales are seasonal: **monthly trends** show peaks in specific periods that could inform inventory planning.
- 🛒 Products like **Chai and Chang** are frequently ordered, which may indicate strong product-market fit.
- 🚚 Multiple orders use **Standard shipping**, suggesting customer preference or cost-efficiency strategy.
- 👨‍💼 Employee order volume varies—insightful for assessing **staff performance and workloads**.


## 💡 7. Sample SQL Queries

```sql


/*Write a SQL query to retrieve all columns from the Customers table.*/
Select *
from customers;

/*Write a SQL query to retrieve all columns from the Customers table where the Country is either 'USA' or 'UK'.*/
select *  
from customers  
where Country  
in("usa","uk"); 

/*Write a query to list the employees who handled each order, along with the order date.*/
select firstname,lastname,orders.orderid,orders.orderdate 
from employees 
inner join orders 
on orders.employeeid = employees.employeeid;

/*Write a query to find all orders shipped by a specific shipper (e.g., ShipperName = 'United Package').*/
SELECT * FROM orders 
INNER JOIN shippers 
ON orders.shipperid = shippers.shipperid 
WHERE ShipperName = 'United Package';

/* alisaing*/
SELECT c.CustomerName, o.OrderID
FROM Customers as c -- rename as c
LEFT JOIN Orders as o -- rename as o
ON c.CustomerID = o.CustomerID
where orderid is null;

/*The following SQL statement lists the number of orders sent by each shipper:*/
SELECT Shippers.ShipperName, COUNT(Orders.OrderID) AS NumberOfOrders FROM Orders
LEFT JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
GROUP BY ShipperName;

/*Write a query to list each product category and the total quantity of products sold in that category.*/
select categoryname, sum(quantity) as totalquanity
from categories as c
join products as p
on c.categoryid = p.categoryid
join order_details as od
on od.productid = p.productid
group by categoryname;

/*Write a SQL query to retrieve all columns from the Customers table where the Country is 'USA' and City
 is either 'Portland' or 'Kirkland', ordered by ascending CustomerName.*/
select *  
from customers  
where Country = "usa" and City in( "portland","kirkland")  
order by CustomerName ; 

 /*Write a query to list each employee and the number of orders they have handled.*/
select firstname, lastname, count(orderid) as totalorders
from employees as e
join orders as o
on e.employeeid = o.employeeid
group by firstname, lastname;


/*Write a query to calculate the price*qty, and name it as Sales*/
select p.productname, p.price, od.quantity, od.quantity*P.price as sales
from products as p
inner join order_details as od
on p.ProductID = od.ProductID;

/*Write a query to total orders by sales display in desending order*/
select  p.productname, sum(od.quantity * p.price) as sales
from products as p
inner join order_details as od
on p.productid = od.productid
group by p.productname
order by sales desc;


