# MySQL-Quaries

/*!40101 SET NAMES utf8 */;

/*!40101 SET SQL_MODE=''*/;

/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
use classicmodels;

-- 1st Question select, where,and,distint.

select * from employees;
select employeeNumber,firstName,lastName from employees where reportsTo = 1102 And JobTitle = "Sales Rep"; 

use classicmodels;

-- 2nd Question Case Statement 
select customerNumber,customerName, 
case
when country in("USA","Canada") then "North America"
when country in("UK","France","germany") then "Europe"
else "Other"
end Customer_Segment
from Customers;

-- 3rd Question Group by(a)

select productCode,Sum(quantityOrdered) as total_ordered from orderdetails group by productCode order by total_ordered desc limit 10;


-- 3rd Question Group by(b)
use classicmodels;
select DATE_FORMAT(paymentDate, '%M') AS month_name ,Count(*) as num_payments from payments group by month_name having num_payments > 20 order by num_payments desc ;



-- 4th question Constrain keys (a)

create database assignment;
create table Customers_Orders (customer_id int primary key auto_increment, 
								first_name varchar(50) not null, last_name varchar(50) not null, 
                                email varchar(255) unique key not null,
                                phone_number varchar(20));
        
        
-- (b)

create table orders_ (order_id int primary key auto_increment,
					customer_id int not null,
                    order_date date,
                    total_amount decimal(10,2),
                    foreign key (customer_id) references Customers_Orders (customer_id));


-- 5th Question Joins

select country, count(orderNumber) as order_count 
from orders inner join Customers on customers.customerNumber = orders.customerNumber
group by country 
order by orderNumber desc limit 5;


-- 6th Question Self join 

use assignment;
create table project (EmployeeId int primary key auto_increment,
					  FullName varchar(50) not null,
                      gender  enum ("Male","female"),
                      ManagerID int);
                      
insert into project (EmployeeID,FullName,gender,ManagerID) values (1, 'Pranaya', 'Male', 3),
(2, 'Priyanka', 'Female', 1),
(3, 'Preety', 'Female', NULL),
(4, 'Anurag', 'Male', 1),
(5, 'Sambit', 'Male', 1),
(6, 'Rajesh', 'Male', 3),
(7, 'Hina', 'Female', 3);
					
select emp.FullName,mgr.FullName from project as emp inner join project as mgr on emp.EmployeeID = mgr.EmployeeID;


-- 7th Question DDL commands 
create table facility  (facility_ID int, name Varchar (50), state varchar (30), country varchar(50));
alter table facility modify column facility_ID int, add primary key auto_increment (Facility_ID);
alter table facility add column city varchar (100) not null after country;
desc  facility;

-- 9th Question Views

Create view product_category_sales  as
select productlines.productLine as Product_line,
sum(orderdetails.quantityOrdered * orderdetails.priceEach) as total_sales,
count(distinct orders.orderNumber) as total_num_of_Sales from productlines
join
products on products.productLine = productlines.productline
join
orderdetails on products.productCode =orderdetails.productCode
join
orders on  orderdetails.orderNumber = orders.orderNumber 
group by productlines.productLine;

select * from product_category_sales;


-- 9th Question Stored Procedure (Get_Country_Payments)

-- 10th Question Windows Function (a)

select customers.customerName, count(orders.orderNumber) as Order_Count, rank() over (order by orders.orderNumber) as order_frequency_rnk
from customers join Orders on customers.customerNumber = Orders.customerNumber
group by customers.customerName
order by Order_count desc;


-- 10th Question Windows Function (b)

select date_format(orderdate, "%Y") as Year_,
 date_format(orderdate, "%M") as Month_, 
 count(orderNumber) as Count_orderNumber, 
 concat(format((LAG(orderNumber) over (order by orderdate) -count(orderNumber) / LAG(orderNumber) over (order by orderdate)) *100,0), "%")  as _YoY_Changes  
 from orders group by year_,Month_;


-- 11th Question SelfJoinb
select productline, buyPrice as Total from products group by productline having Total > (select avg(buyPrice) from products);

-- 12th Question Error Handling in Stored Porocedures

-- 13th Question Triggers

use Assignment;
CREATE TABLE Emp_BIT (
    Name VARCHAR(50),
    Occupation VARCHAR(50),
    Working_date DATE,
    Working_hours INT
);

CREATE TABLE Emp_BIT2  (
    Name VARCHAR(50),
    Occupation VARCHAR(50),
    Working_date DATE,
    Working_hours INT
    );
    
    
    
INSERT INTO Emp_BIT (Name, Occupation, Working_date, Working_hours) VALUES
('Robin', 'Scientist', '2020-10-04', 12),  
('Warner', 'Engineer', '2020-10-04', 10),  
('Peter', 'Actor', '2020-10-04', 13),  
('Marco', 'Doctor', '2020-10-04', 14),  
('Brayden', 'Teacher', '2020-10-04', 12),  
('Antonio', 'Business', '2020-10-04', 11);


select * from Emp_BIT2;


DROP TABLE Emp_BIT;
DROP TABLE Emp_BIT2;
