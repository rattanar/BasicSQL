Welcome to the mySQL wiki!

SQL: Structured Query Language

[w3school online console](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all)

*****************************
## **CREATE TABLE**

* สร้างตาราง(table) ชื่อ "Students" โดยโครงสร้างประกอบไปด้วย 
* Column "StudentID" type int (ค่านี้สามารถใช้เป็น primary key(unique) ได้)
* Column "StudentName" type varchar(string) max length 255
* Column "University" type varchar(string) max length 255
* Column "City" type varchar(string) max length 255

````

CREATE TABLE Students(

StudentID int, 
StudentName varchar(255),
University varchar(255),
City varchar(255)
);

````

**INSERT** data into table

````
INSERT INTO Students VALUES(1, "Judy Hopps", "University of Bangkok", "Bangkok");
````
or more readable,
````
INSERT INTO Students(StudentID,StudentName,University,City) VALUES(2,"Nick Wilde","CU","Bangkok")
INSERT INTO Students(StudentID,StudentName,University,City) VALUES(3,"Mister Big","KMITL","Bangkok");
````

**DELETE** a row

````
DELETE FROM Students WHERE University = "CU";
````

**UPDATE** a row

````
UPDATE Students SET University = "KU" WHERE StudentID = 1;
````

**DROP**, Delete a table

````
DROP TABLE Students;
````

*****************************

**SELECT**
`SELECT <column_name1,column_name2,...> FROM <table_name>;`

````
SELECT CustomerName,City FROM Customers;
SELECT * FROM Customers;
````
*****************************

**SELECT DISTINCT Statement**
`SELECT DISTINCT column_name,column_name FROM table_name;`

คีย์เวิร์ด DISTINCT จะเลือกเฉพาะข้อมูลที่ไม่เหมือนกันออกมาจากตาราง ข้อมูลไหนที่ถูกเลือกมาแต่ค่าซ้ำกันจะถูกตัดทิ้งและแสดงผลแค่ค่าเดียว 

````
SELECT DISTINCT City FROM Customers;
SELECT DISTINCT City,PostalCode FROM Customers;
````
*****************************

**WHERE** Clause
`SELECT column_name,column_name FROM table_name WHERE column_name operator value;`

Operators
`=`: เลือกเฉพาะค่าที่เท่ากับ

`<>`: เลือกเฉพาะค่าที่ไม่เท่ากับ 

`>`,`<`, `>=`, `<=`: มากกว่า,น้อยกว่า,มากว่าหรือเท่ากับ,น้อยกว่าหรือเท่ากับ

`BETWEEN...AND...`: ใช้สำหรับกำหนดเงื่อนไขในการค้นหาค่าที่อยู่ระหว่างค่าๆหนึ่ง กับค่าๆหนึ่ง

`IN`: ใช้ตรวจสอบและดึงค่าหลายๆรูปแบบสำหรับคอลั่มใดๆ

`LIKE`: ค้นหา pattern

`AND` / `OR` operators 
ใช้กรองค่าด้วยเงื่อนไขหลายๆรูปแบบ

````
SELECT * FROM Customers WHERE City='London';
SELECT CustomerName,City FROM Customers WHERE CustomerID BETWEEN 10 AND 30;
SELECT CustomerName,City FROM Customers WHERE CustomerID NOT BETWEEN 10 AND 30;
SELECT * FROM Customers WHERE Country IN('Germany','UK','Sweden');
SELECT * FROM Customers WHERE Country = 'UK' AND City = 'London';
SELECT * FROM Customers WHERE Country = 'UK' OR Country = 'Germany';
````
*****************************

**LIKE** Operator and **Wildcards**
wildcard character ใช้เป็นตัวแทนของตัวอักษรใดๆใน string เพื่อค้นหาข้อมูลในตารางร่วมกับ **LIKE** operator


`%`: ตัวอักษรตั้งแต่ 0 ตัวขึ้นไป (A substitute for zero or more characters)
`_`: ตัวอักษร 1 ตัว (a single character)

ค้นหาบุคคลที่ชื่อนำหน้าด้วยคำว่า Antonio
````
SELECT * FROM Customers WHERE ContactName LIKE "Antonio%";
````

ดึงข้อมูลเฉพาะเมืองที่มีคำว่า "es" อยู่ด้วย 
````
SELECT * FROM Customers WHERE City LIKE '%es%';
````

ค้นหาบุคคลที่ชื่อยาว 7 ตัวอักษร
````
SELECT * FROM Customers WHERE ContactName LIKE "_______";
````

ค้นหาเมืองด้วยรูปแบบต่างๆ
````
SELECT * FROM Customers WHERE City LIKE 'L_n_on';
````

????????????????????????????????
wildcard 
http://www.w3schools.com/sql/sql_wildcards.asp
`[charlist]`: กลุ่มของตัวอักษรเพื่อค้นหา (Sets and ranges of characters to match)
[^charlist] หรือ [!charlist]: ค้นหาเฉพาะข้อมูลที่ไม่ตรงกับ charlist (Matches only a character NOT specified within the brackets)
????????????????????????????????

*****************************

**SQL built-in Function**

มีสองลักษณะคือ 1.Aggregate Functions 2.Scalar functions

**_Aggregate Functions_** คือฟังก์ชันที่จะคืนค่าเป็นค่าๆเดียวซึ่งได้มาจากการป้อนด้วยอินพุตแบบคอลั่ม ตัวอย่างเช่น
(return a single value, calculated from values in a column.)
* AVG() - ใช้สำหรับหาค่าเฉลี่ยในฐานข้อมูลจากฟิลด์ที่เป็นตัวเลข
* COUNT() - ใช้สำหรับนับจำนวนแถวในตาราง
* FIRST() - ใช้สำหรับส่งกลับค่าแรกของคอลัมน์ที่เลือก
* MAX() - ใช้สำหรับส่งกลับค่าที่มีค่ามากสุด
* MIN() - ใช้สำหรับส่งกลับค่าที่มีค่าน้อยสุด
* SUM() - ใช้สำหรับบอกผลรวม
* LAST() - ใช้สำหรับคืนค่าแถวท้ายสุด

นับจำนวนคอลั่มทั้งหมดที่ชื่อเมืองอยู่ในรูปแบบ "Lon%"
````
SELECT COUNT(*) FROM Customers WHERE City LIKE "Lon%";
````
สามารถตั้งชื่อตัวแปรมารับค่าได้ด้วย "AS" ในที่นี้ตั้งชื่อว่า countResult หรือสามารถตั้งชื่อไปเลยว่า countResult เลยก็ได้เช่นกัน ค่าที่แสดงจะมีค่าเหมือนกัน
````
SELECT COUNT(*) AS countResult FROM Customers WHERE City LIKE "Lon%";
SELECT COUNT(*) countResult FROM Customers WHERE City LIKE "Lon%";
````

หาผลรวม/ค่าเฉลี่ย/ค่าที่สูงสุด/ต่ำสุด ของ CustomerID ของแถวข้อมูลทั้งหมดที่ชื่อเมืองอยู่ในรูปแบบ "Lon%"
````
SELECT SUM(CustomerID) FROM Customers WHERE City LIKE "Lon%";
SELECT AVG(CustomerID) FROM Customers WHERE City LIKE "Lon%";
SELECT MAX(CustomerID) FROM Customers WHERE City LIKE "Lon%";
SELECT MIN(CustomerID) FROM Customers WHERE City LIKE "Lon%";
````


**_Scalar functions_** คือฟังก์ชันที่จะคืนค่าเป็นค่าๆเดียวซึ่งได้มาจากการป้อนอินพุตค่าๆหนึ่งเข้าไป ตัวอย่างเช่น
(return a single value, based on the input value.)
* UCASE() or UPPER() - ใช้แปลงตัวอักษรเป็นตัวพิมพ์ใหญ่ 
* LOWER() or LCASE() - ใช้แปลงตัวอักษรเป็นตัวพิมพ์เล็ก
* MID() - ใช้สำหรับตัดคำในฟิลด์ออกมาเท่าที่ต้องการ
* LEN() - ใช้สำหรับดูความยาวของข้อความ
* ROUND() - ใช้สำหรับปัดเศษทศนิยมของตัวเลข
* NOW() - ใช้สำหรับแสดงวันที่และเวลาของระบบปัจจุบัน
* FORMAT() - ใช้ในการจัดรูปแบบวิธีการที่เขตข้อมูลเป็นที่จะแสดง

*****************************

**ORDER BY**
`SELECT column_name, column_name
FROM table_name
ORDER BY column_name ASC|DESC, column_name ASC|DESC;`

````
SELECT * FROM Customers ORDER BY City;
SELECT * FROM Customers ORDER BY City DESC;
SELECT * FROM Customers ORDER BY Country, CustomerName;
SELECT * FROM Customers ORDER BY Country ASC, CustomerName DESC;
````

*****************************

**JOIN** Clause

JOIN clause is used to combine rows from two or more tables.
Types of the different SQL JOINs

1. **INNER JOIN**: is the same as JOIN, returns all rows when there is at least one match in BOTH tables
2. **LEFT OUTER JOIN**: is the same as LEFT JOIN, return all rows from the left table, and the matched rows from the right table
3. **RIGHT OUTER JOIN**: is the same as RIGHT JOIN, return all rows from the right table, and the matched rows from the left table
4. **FULL OUTER JOIN**: Return all rows when there is a match in ONE of the tables

----------------------------

1. **INNER JOIN**
To returns selection data from Order and Customer tables when condition matching then

![inner join sql](https://github.com/Napat/BasicSQL/blob/master/img/img_innerjoin.gif)

````
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate 
FROM Orders INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;

SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate 
FROM Orders JOIN Customers ON Orders.CustomerID=Customers.CustomerID;

SELECT o.OrderID, c.CustomerName, o.OrderDate 
FROM Orders AS o JOIN Customers AS c ON o.CustomerID=c.CustomerID;
````

----------------------------

2. **LEFT OUTER JOIN**

![sql left join](https://github.com/Napat/BasicSQL/blob/master/img/img_leftjoin.gif)

````
SELECT Customers.CustomerName, Orders.OrderID FROM Customers
LEFT JOIN Orders ON Customers.CustomerID=Orders.CustomerID
ORDER BY Orders.OrderID;
````

----------------------

3. **RIGHT OUTER JOIN**

![Right join](https://github.com/Napat/BasicSQL/blob/master/img/img_rightjoin.gif)

````
SELECT Orders.OrderID, Employees.FirstName FROM Orders
RIGHT JOIN Employees ON Orders.EmployeeID=Employees.EmployeeID
ORDER BY Employees.FirstName;
````

----------------------

4. **FULL OUTER JOIN**

![full join](https://github.com/Napat/BasicSQL/blob/master/img/img_fulljoin.gif)

````
SELECT Customers.CustomerName, Orders.OrderID FROM Customers
FULL OUTER JOIN Orders ON Customers.CustomerID=Orders.CustomerID
ORDER BY Orders.OrderID;
````

----------------------

*****************************

**SQL UNION**
ใช้ยูเนี่ยนเพื่อรวมเอาผลลัพธ์จาก select หลายๆอันเข้าด้วยกัน
โดย default การยูเนี่ยนจะตัดผลัพธ์ที่ซ้ำค่ากันออกไป 
คีย์เวิรด์ ALL ใช้เมื่อต้องการผลลัพธ์ที่มีการซ้ำค่ากันด้วย


````
SELECT City FROM Customers UNION SELECT City FROM Suppliers ORDER BY City;

SELECT City FROM Customers UNION ALL SELECT City FROM Suppliers ORDER BY City;

SELECT City, Country FROM Customers WHERE Country='Germany' 
UNION ALL 
SELECT City, Country FROM Suppliers WHERE Country='Germany' 
ORDER BY City;
````

*****************************

**SQL GROUP BY** Statement  
`SELECT column_name, aggregate_function(column_name) FROM table_name WHERE column_name operator value GROUP BY column_name;`

การนับค่าประเทศต่างๆอาจจะสามารถทำได้โดย

````
SELECT COUNT(*) FROM Customers WHERE Country = "Germany";
SELECT COUNT(*) FROM Customers WHERE Country = "USA";
SELECT COUNT(*) FROM Customers WHERE Country = "UK";
...
````

แต่วิธีดังกล่าวไมมีประสิทธิภาพนัก และเป็นการเขียนโค๊ดซ้ำไปซ้ำมา
เราสามารถใช้ GROUP BY เข้ามาช่วยนับค่าได้

````
SELECT COUNT(*), Country FROM Customers GROUP BY Country;
SELECT COUNT(*) AS NumOfCustomers, Country FROM Customers GROUP BY Country;
SELECT COUNT(*) AS NumOfCustomers, Country FROM Customers GROUP BY Country ORDER BY NumOfCustomers DESC;
````

โจทย์: แสดงค่า ชื่อลูกค้าและจำนวณ orders ของแต่ละคน
1. จัดกลุ่มด้วยลูกค้าแต่ละคน(ใช้ CustomerName หรือ CustomerID ก็ได้) ``GROUP BY c.CustomerName`` หรือ ``GROUP BY o.CustomerID``
1. จากข้อมูลของตาราง Customers และ INNER JOIN กับข้อมูลในตาราง Orders ที่มีค่า CustomerID เหมือนกันทั้งสองตาราง
1. ดึงค่า ชื่อลูกค้า และ นับจำนวนผู้ใช้ ``SELECT c.Country, COUNT(o.CustomerID)`` จากการจัดกลุ่มตามชื่อหรือเลขผู้ใช้งานออกมาแสดงผล
1. เรียงลำดับจากมากไปน้อย

````
SELECT c.CustomerName, COUNT(o.CustomerID) AS NumOfOrders 
FROM Customers AS c INNER JOIN Orders AS o ON o.CustomerID = c.CustomerID
GROUP BY c.CustomerName
ORDER BY NumOfOrders DESC;

SELECT c.CustomerName, COUNT(o.CustomerID) AS NumOfOrders 
FROM Customers AS c INNER JOIN Orders AS o ON o.CustomerID = c.CustomerID
GROUP BY o.CustomerID 
ORDER BY NumOfOrders DESC;
````

โจทย์: แสดงค่าจำนวณ orders ของแต่ละประเทศ
1. จับกลุ่มข้อมูลตามชื่อประเทศ ``GROUP BY c.Country``
1. จากข้อมูลของตาราง Customers และ INNER JOIN กับข้อมูลในตาราง Orders ที่มีค่า CustomerID เหมือนกันทั้งสองตาราง
1. ดึงค่า ชื่อประเทศ และ นับจำนวนผู้ใช้ ``SELECT c.Country, COUNT(o.CustomerID)`` จากการจัดกลุ่มด้วยชื่อประเทศออกมาแสดงผล 
1. เรียงลำดับจากมากไปน้อย

````
SELECT c.Country, COUNT(o.CustomerID) AS NumOfOrders FROM Customers AS c
INNER JOIN Orders AS o ON o.CustomerID = c.CustomerID
GROUP BY c.Country
ORDER BY NumOfOrders DESC;
````


????
````
SELECT Shippers.ShipperName,COUNT(Orders.OrderID) AS NumberOfOrders FROM Orders LEFT JOIN Shippers ON Orders.ShipperID=Shippers.ShipperID GROUP BY ShipperName;

SELECT Shippers.ShipperName, Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders FROM ((Orders INNER JOIN Shippers ON Orders.ShipperID=Shippers.ShipperID) INNER JOIN Employees ON Orders.EmployeeID=Employees.EmployeeID) GROUP BY ShipperName,LastName;
````
????

*****************************

**HAVING** Clause

The HAVING clause was added to SQL because the WHERE keyword could not be used with aggregate functions.

`SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders FROM (Orders
INNER JOIN Employees
ON Orders.EmployeeID=Employees.EmployeeID)
GROUP BY LastName
HAVING COUNT(Orders.OrderID) > 10;`

````
SELECT Country, COUNT(*) AS CustomerNum FROM Customers GROUP BY Country HAVING CustomerNum > 5 ORDER BY CustomerNum DESC;
````


????????????????????
````
SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders FROM (Orders
INNER JOIN Employees
ON Orders.EmployeeID=Employees.EmployeeID)
GROUP BY LastName
HAVING COUNT(Orders.OrderID) > 10;

SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders FROM Orders
INNER JOIN Employees
ON Orders.EmployeeID=Employees.EmployeeID
WHERE LastName='Davolio' OR LastName='Fuller'
GROUP BY LastName
HAVING COUNT(Orders.OrderID) > 25;
````
???????????????????????????????

*****************************

**Subquery** หรือ **Inner query** หรือ **Nested query**
คือการคิวรี่ข้อมูลซ้อนเข้าไปใน WHEER clause ของ SQL query อีกชั้นหนึ่ง


โจทย์: จากข้อมูลตาราง Customers จงแสดงผลค่า CustomerID ที่มีตัวมากกว่าค่าเฉลี่ยทั้งหมด
````
SELECT * FROM Customers WHERE CustomerID > (
 SELECT AVG(CustomerID) FROM Customers
)
````

*****************************
