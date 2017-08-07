

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
mysql> CREATE DATABASE userorder;
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| userorder          |
+--------------------+
5 rows in set (0.00 sec)
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



