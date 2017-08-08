

# การติดตั้ง mysql-server และเข้าสู่ MySQL shell
## Ubuntu  
```
sudo apt-get install mysql-server
mysql -u root -p
```

## Centos  
```
sudo yum install mysql-server
/etc/init.d/mysqld start
mysql -u root -p
```

## [Docker](https://hub.docker.com/r/mysql/mysql-server/)
```
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=password -d mysql/mysql-server:5.7
docker exec -it mysql-container bash
mysql -u root -p
```

## วิธีถอนการติดตั้งใน ubuntu
```
sudo apt-get remove --purge mysql*
sudo apt-get autoremove
sudo apt-get autoclean
```

----------------------------------------------------

# Begin Tutorial  
- คำสั่ง sql นั้นเป็น case-insensitive แปลว่า ไม่ว่าคำสั่งจะเป็นอักษรตัวใหญ่หรือตัวเล็กก็ให้ความหมายแบบเดียวกัน
- sql statement จะจบด้วยเครื่องหมาย `;` เสมอ

## คำสั่งตรวจสอบ database ที่มีอยู่ในเครื่องทั้งหมด 
```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
```

## สร้าง(Create) database ใหม่ขึ้นมา
```
mysql> CREATE DATABASE events;
mysql> CREATE DATABASE userorder;
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| events             |
| mysql              |
| performance_schema |
| sys                |
| userorder          |
+--------------------+
```

## การลบ(drop) database  
```
mysql> drop database userorder;
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| events             |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```

## ทำความรู้จัก type ต่างๆของข้อมูล  
Data types ของ database ต่างๆ(MySQL, Microsift Access, SQL Server, ...) จะแตกต่างกันไปในรายละเอียด เช่น VARCHAR ใน MySQL จะมีขนาด 1byte/char แต่ใน SQLServer จะมีขนาด 2bytes/char ดังนั้นควรตรวจสอบให้ดีก่อนใช้งาน   

### MySQL data types  
ในที่นี้จะยกตัวอย่างประเภทข้อมูลพื้นฐาน 3 ประเภทเบื้องต้นสำหรับฐานข้อมูล MySQL ดังนี้ 1.ตัวหนังสือ(text), 2.ตัวเลข(number) และ 3. เวลา(Date/Time)  
#### 1.Text types
- CHAR  
    เก็บข้อมูลขนาด 1byte/1ตัวอักษร ใช้เก็บข้อมูลตัวอักษรที่มีขนาดแน่นอนเสมอ เช่น 
    ออกแบบฐานข้อมูลไว้ 10 char แต่ถ้าข้อมูลใน Field มี 5 ก็จะใช้พื้นที่เก็บ 10 char(data + space 5 char)
    สรุปแบบสั้นๆคือเป็นการเก็บตัวอักษรแบบ fixed length   
- VARCHAR  
    เก็บข้อมูลขนาด 1byte/1ตัวอักษร ใช้กับข้อมูลตัวอักษรที่ไม่มีขนาดแน่นอนไม่สามารถกำหนดได้ตรงตัว 
    จะเก็บข้อมูลตามจริงเช่น ออกแบบไว้ 10 char แต่ถ้าข้อมูลใน Field มี 5 ก็จะเก็บข้อมูลแค่ 5 char 
    สรุปแบบสั้นๆคือเป็นการเก็บตัวอักษรแบบ variable length   
#### 2.Number types  
- INT  
    เก็บข้อมูลตัวเลขในช่วง -2147483648 to 2147483647 และ 0 ถึง 4294967295 สำหรับตัวแปรชนิด UNSIGNED  
#### 3.Date types
- DATE  
    เก็บข้อมูลวันที่ในรูปแบบ YYYY-MM-DD ข้อมูลที่รองรับจะต้องอยู่ในช่วง '1000-01-01' ถึง '9999-12-31'  
- DATETIME  
    เก็บข้อมูลวันที่ในรูปแบบ YYYY-MM-DD HH:MI:SS ข้อมูลที่รองรับจะต้องอยู่ในช่วง 1000-01-01 00:00:00' ถึง '9999-12-31 23:59:59'  
- TIMESTAMP  
    ข้อมูล timestamp หน่วยวินาทีในรูปแบบ YYYY-MM-DD HH:MI:SS ตามมาตรฐาน Unix epoch (เริ่มนับเวลาตั้งแต่ '1970-01-01 00:00:00' UTC)  
    รองรับช่วงเวลาตั้งแต่ '1970-01-01 00:00:01' UTC ถึง '2038-01-09 03:14:07' UTC  
**Note** datatype ของ MySQL มีอีกหลายชนิดสามารถดูเพิ่มเติมได้ที่ [mysqlRef5.7](https://dev.mysql.com/doc/refman/5.7/en/data-types.html)  

## Primary key  
primary key คือ field หรือ column ที่ทำให้ข้อมูลแต่ละแถว(row/record)มีความ unique นั่นคือค่าที่เก็บจะไม่ซ้ำกันในแถวของ primary key นั่นเอง อีกทั้งค่าจะต้องไม่เท่ากับ NULL   
primary key สามารถสร้างจาก field เดียวหรือเกิดจากการรวมกันจากหลายๆ field(composite primary key) ก็ได้ โดยเราสามารถกำหนด primary key ได้ทั้งจากขั้นตอนการสร้าง table(bset practices) หรือ alter    

## เริ่มการ access database และแสดง table ที่มีอยู่ใน database ที่เลือกใช้งาน
```
mysql> USE events;
mysql> SHOW TABLES;
Empty set (0.00 sec)
```
เนื่องจากเป็น database ที่พึ่งสร้างขึ้นมาใหม่จึงยังไม่มี table อะไรอยู่เลย(Empty set)  

## การสร้าง table  
```
CREATE TABLE persons
(
id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
first_name varchar(20) NOT NULL,
last_name varchar(20) NOT NULL,
address varchar(255),
birth DATE,
sex CHAR(1)
);
mysql>
mysql> SHOW tables;
```
1. เราได้ทำการสร้าง table ที่ชื่อว่า persons ซึ่งประกอบไปด้วย 6 column สำหรับเก็บข้อมูล  
2. `id` จะถูกใช้เป็น primary key ซึ่งค่าจะถูกเพิ่มขึ้นโดยอัตโนมัติเมื่อใส่ข้อมูลลงไปใน table
3. `first_name` และ `last_name` ใช้เก็บตัวอักษรได้สูงที่สุดเท่ากับ 20char และห้ามมีค่าเป็น NULL  
4. `address` เป็นตัวอักษรความยาวสูงสุด 255char  
5. `birth` เก็บค่าวันเกิดใรูปแบบ YYYY-MM-DD  
6. `sex` เ็บตัวอีกษร M หรือ F เพื่อระบุเพศ  

```
mysql> SHOW TABLES;
+------------------+
| Tables_in_events |
+------------------+
| persons          |
+------------------+
```

## คำสั่งแสดงโครงสร้างของตาราง  
```
mysql> DESCRIBE persons;
+------------+--------------+------+-----+---------+----------------+
| Field      | Type         | Null | Key | Default | Extra          |
+------------+--------------+------+-----+---------+----------------+
| id         | int(11)      | NO   | PRI | NULL    | auto_increment |
| first_name | varchar(20)  | NO   |     | NULL    |                |
| last_name  | varchar(20)  | NO   |     | NULL    |                |
| address    | varchar(255) | YES  |     | NULL    |                |
| birth      | date         | YES  |     | NULL    |                |
| sex        | char(1)      | YES  |     | NULL    |                |
+------------+--------------+------+-----+---------+----------------+
```

## การเพิ่มข้อมูล(INSERT)ลงใน database  
```
mysql> INSERT INTO `persons` 
(`id`,`first_name`,`last_name`,`address`,`birth`,`sex`) 
VALUES  
(NULL, "Tony", "Stark", "10880 Malibu Point, 9026", "1970-05-29", "M");

mysql> INSERT INTO `persons` 
(`id`,`first_name`,`last_name`,`address`,`birth`,`sex`) 
VALUES  
(NULL, "Steve", "Rogers", NULL, "1920-07-04", "M");
 
mysql> INSERT INTO `persons` 
(`id`,`first_name`,`last_name`,`address`,`birth`,`sex`) 
VALUES  
(NULL, "Peter", "Parker", "The Parker House, New York City", NULL, "M");

```

## การ SELECT ข้อมูลจากตารางเบื้องต้น
```
mysql> SELECT * FROM persons;
+----+------------+-----------+---------------------------------+------------+------+
| id | first_name | last_name | address                         | birth      | sex  |
+----+------------+-----------+---------------------------------+------------+------+
|  1 | Tony       | Stark     | 10880 Malibu Point, 9026        | 1970-05-29 | M    |
|  2 | Steve      | Rogers    | NULL                            | 1920-07-04 | M    |
|  3 | Peter      | Parker    | The Parker House, New York City | NULL       | M    |
+----+------------+-----------+---------------------------------+------------+------+
```

## การเปลี่ยนแปลงข้อมูล(UPDATE)ที่มีอยู่ในตาราง  
```
mysql> UPDATE `persons` 
SET `address` = "Washington DC apartment address: 1614 Connecticut Ave NW" 
WHERE `persons`.`last_name`="Rogers";

mysql> SELECT * FROM persons;
+----+------------+-----------+----------------------------------------------------------+------------+------+
| id | first_name | last_name | address                                                  | birth      | sex  |
+----+------------+-----------+----------------------------------------------------------+------------+------+
|  1 | Tony       | Stark     | 10880 Malibu Point, 9026                                 | 1970-05-29 | M    |
|  2 | Steve      | Rogers    | Washington DC apartment address: 1614 Connecticut Ave NW | 1920-07-04 | M    |
|  3 | Peter      | Parker    | The Parker House, New York City                          | NULL       | M    |
+----+------------+-----------+----------------------------------------------------------+------------+------+
3 rows in set (0.00 sec)
```

## การแก้ไข(ALTER): เพิ่ม(ADD)/ลบ(DROP) column ของตาราง(Table)  
```
mysql> ALTER TABLE persons ADD Status VARCHAR(10);
mysql> ALTER TABLE persons DROP Status;
mysql> ALTER TABLE persons ADD Status VARCHAR(10) AFTER birth;
```

## การลบข้อมูลแถวใดๆ(Delete a Row)  
```
mysql> DELETE from persons  where first_name='Steve';
mysql> SELECT * FROM persons;
+----+------------+-----------+---------------------------------+------------+--------+------+
| id | first_name | last_name | address                         | birth      | Status | sex  |
+----+------------+-----------+---------------------------------+------------+--------+------+
|  1 | Tony       | Stark     | 10880 Malibu Point, 9026        | 1970-05-29 | NULL   | M    |
|  3 | Peter      | Parker    | The Parker House, New York City | NULL       | NULL   | M    |
+----+------------+-----------+---------------------------------+------------+--------+------+
2 rows in set (0.00 sec)
```

