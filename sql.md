

## Constraints
- **NOT NULL**: It ensures that a column cannot have a NULL value
- **UNIQUE**: It ensures that a column accepts only UNIQUE values
- **DEFAULT**: It provides a default value for a column when none is specified
- **PRIMARY KEY** : 
    - It ensures that a column accepts only UNIQUE value 
    - there can be a single PRIMARY Key on a table but multiple columns can constitute a PRIMARY Key
- **FOREIGN KEY REFERENCES**: It  maps with a column in another table and uniquely identifies a row/record in that table
- **CHECK**: check the validity of the data entered into the particular column.
- **INDEX **: 
    - created to speed up the data retrieval from the database. 
    - An Index can be created by using a single or group of columns in a table. 
    - A table can have a single PRIMARY Key but can have multiple INDEXES.
    - `CREATE INDEX idx_age ON CUSTOMERS ( AGE );` 
**NOTE : UNIQUE , PRIMARY KEY , FOREIGN KEY are data integrity constraints , they Referencial Integrity.**



``` sql
CREATE TABLE CUSTOMERS (
   ID INT NOT NULL UNIQUE,
   NAME VARCHAR (20) DEFAULT 'Not Available',
   AGE INT NOT NULL CHECK(AGE>=18),
   ADDRESS CHAR (25),
   SALARY DECIMAL (18, 2)
);

CREATE INDEX idx_age ON CUSTOMERS ( AGE );
```
```sql
CREATE TABLE ORDERS (
   ID INT NOT NULL,
   DATE DATETIME,
   CUSTOMER_ID INT FOREIGN KEY REFERENCES CUSTOMERS(ID),
   AMOUNT DECIMAL,
   PRIMARY KEY (ID)
);
```
- **DDL:** `CREATE` , `ALTER`  `TABLE` , `DROP` , `TRUNCATE` , and `ADD COLUMN` 
- **DML:** `UPDATE` , `DELETE` , and `INSERT` 
- **DCL:** `GRANT`  and `REVOKE` 
- **TCL:** `COMMIT` , `SET TRANSACTION` , `ROLLBACK` , and `SAVEPOINT` 
- **DQL:** – `SELECT` 


## **DDL:**
```sql
/*
   NOT NULL --> feild shouldn't be empty
   PRIMARY KEY --> It should be unique in the entire table
*/


CREATE TABLE CUSTOMERS(
   ID          INT NOT NULL,
   NAME        VARCHAR (20) NOT NULL,
   AGE         INT NOT NULL,
   ADDRESS     CHAR (25),
   SALARY      DECIMAL (18, 2),
   PRIMARY KEY (ID)
);


DESC CUSTOMERS;

/*
---> output
+---------+---------------+------+-----+---------+-------+
| Field   | Type          | Null | Key | Default | Extra |
+---------+---------------+------+-----+---------+-------+
| ID      | int           | NO   | PRI | NULL    |       |
| NAME    | varchar(20)   | NO   |     | NULL    |       |
| AGE     | int           | NO   |     | NULL    |       |
| ADDRESS | char(25)      | YES  |     | NULL    |       |
| SALARY  | decimal(18,2) | YES  |     | NULL    |       |
+---------+---------------+------+-----+---------+-------+
*/


--> Creating table **if not exists**

CREATE TABLE IF NOT EXISTS CUSTOMERS(
   ID          INT NOT NULL,
   NAME        VARCHAR (20) NOT NULL,
   AGE         INT NOT NULL,
   ADDRESS     CHAR (25),
   SALARY      DECIMAL (18, 2),
   PRIMARY KEY (ID)
);


--> Creating table from existing table

CREATE TABLE  dummy_customer as select * from CUSTOMERS;

--> Renaming a table 

-> RENAME TABLE CUSTOMERS to BUYERS;

-> ALTER TABLE BUYERS RENAME TO CUSTOMERS;


--> Truncating the table 

TRUNCATE TABLE CUSTOMERS;

--> Drop the table 

DROP TABLE CUSTOMERS; --> If table exists it throws error

DROP TABLE IF EXISTS CUSTOMERS;

--> Delete

DELETE FROM CUSTOMERS;

DELETE FROM CUSTOMERS 
WHERE NAME='Komal' OR ADDRESS='Mumbai';
```
**Truncate **:

-  Delete the whole data of the table
- Its DDL command
- It requires table alter permission
- table space is not completely freed
**Drop **: 

- Remove the entire table along with its definition
- its DDL Command
- Table space is completely freed
- DROP command is much slower than TRUNCATE but faster than DELETE.
**Delete **: 

- Deletes the data from the table 
- It can also remove pieces of data from a table using where conditions
- requires permissions
- It takes lot of transactional space compared to the trunk




## DML 
```sql
INSERT INTO CUSTOMERS (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (1, 'Ramesh', 32, 'Ahmedabad', 2000.00 );

INSERT INTO CUSTOMERS (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (2, 'Khilan', 25, 'Delhi', 1500.00 );

INSERT INTO CUSTOMERS (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (3, 'Kaushik', 23, 'Kota', 2000.00 );


--> we can also insert multiple rows
INSERT INTO CUSTOMERS (ID,NAME,AGE,ADDRESS,SALARY) VALUES
(4, 'Chaitali', 25, 'Mumbai', 6500.00 ),
(5, 'Hardik', 27, 'Bhopal', 8500.00 ),
(6, 'Komal', 22, 'Hyderabad', 4500.00 );


--> Backing up the tables
# entire table backup

SELECT * INTO CUSTOMER_BACKUP FROM CUSTOMERS;

# specific columns backup

SELECT name, age, address 
INTO CUSTOMER_DETAILS 
FROM CUSTOMERS;


## Updating ##

--> updating single column
UPDATE CUSTOMERS SET ADDRESS = 'Pune' WHERE ID = 6;

--> updating multiple rows &  columns
UPDATE CUSTOMERS SET AGE = AGE+5, SALARY = SALARY+3000;

## Deleting ##

--> Deleting specific rows
DELETE FROM CUSTOMERS WHERE AGE > 25;

--> Deleting Entire table
DELETE FROM CUSTOMERS;

```




## Views : 
- It is a composition of multiple tables. 
- Unless indexed, a view does not exist in a database.


```sql
CREATE TABLE CUSTOMERS(
   ID          INT NOT NULL,
   NAME        VARCHAR (20) NOT NULL,
   AGE         INT NOT NULL,
   ADDRESS     CHAR (25),
   SALARY      DECIMAL (18, 2),
   PRIMARY KEY (ID)
);

DESC CUSTOMERS;


INSERT INTO CUSTOMERS VALUES
(1, 'Ramesh', 32, 'Ahmedabad', 2000.00 ),
(2, 'Khilan', 25, 'Delhi', 1500.00 ),
(3, 'Kaushik', 23, 'Kota', 2000.00 ),
(4, 'Chaitali', 25, 'Mumbai', 6500.00 ),
(5, 'Hardik', 27, 'Bhopal', 8500.00 ),
(6, 'Komal', 22, 'Hyderabad', 4500.00 ),
(7, 'Muffy', 24, 'Indore', 10000.00 );


--> Creating a view

CREATE VIEW BUYERS_VIEW as SELECT * FROM CUSTOMERS 
WHERE SALARY > 3000;

SELECT * from BUYERS_VIEW;


/*
+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  6 | Komal    |  22 | Hyderabad |  4500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+
*/

--> Updating View
UPDATE CUSTOMERS_VIEW 
SET AGE = 35 WHERE name = 'Ramesh';

--> Veirfying list of views

SELECT TABLE_SCHEMA, TABLE_NAME 
FROM INFORMATION_SCHEMA.VIEWS
WHERE TABLE_SCHEMA='tutorials';

/*
TABLE_SCHEMA	TABLE_NAME
tutorials	CUSTOMERS_VIEW1
tutorials	CUSTOMERS_VIEW2
tutorials	CUSTOMERS_VIEW3

*/

```


## DQL
```sql
CREATE TABLE CUSTOMERS(
   ID          INT NOT NULL,
   NAME        VARCHAR (20) NOT NULL,
   AGE         INT NOT NULL,
   ADDRESS     CHAR (25),
   SALARY      DECIMAL (18, 2),
   PRIMARY KEY (ID)
);

-- DESC CUSTOMERS;


INSERT INTO CUSTOMERS VALUES
(1, 'Ramesh', 32, 'Ahmedabad', 2000.00),
(2, 'Khilan', 25, 'Delhi', 1500.00),
(3, 'Kaushik', 23, 'Kota', 2000.00),
(4, 'Chaitali', 25, 'Mumbai', 6500.00),
(5, 'Hardik', 27, 'Bhopal', 8500.00),
(6, 'Komal', 22, 'Hyderabad', 4500.00),
(7, 'Muffy', 24, 'Indore', 10000.00);



/*
Fetching the ID, NAME and SALARY fields from the 
CUSTOMERS table for the records where the SALARY is greater than 2000;
*/

select ID , NAME , SALARY FROM CUSTOMERS  WHERE SALARY>2000;

+----+----------+----------+
| ID | NAME     | SALARY   |
+----+----------+----------+
|  4 | Chaitali |  6500.00 |
|  5 | Hardik   |  8500.00 |
|  6 | Komal    |  4500.00 |
|  7 | Muffy    | 10000.00 |
+----+----------+----------+



/*
we are incrementing the salary of the customer named Ramesh by 10000
*/

update CUSTOMERS set SALARY=SALARY+10000 where NAME= 'Ramesh';
select * from CUSTOMERS where NAME = 'Ramesh';

/*
+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
|  2 | Khilan   |  25 | Delhi     |  1500.00 |
|  3 | Kaushik  |  23 | Kota      |  2000.00 |
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  6 | Komal    |  22 | Hyderabad |  4500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+
*/

+----+--------+-----+-----------+----------+
| ID | NAME   | AGE | ADDRESS   | SALARY   |
+----+--------+-----+-----------+----------+
|  1 | Ramesh |  32 | Ahmedabad | 12000.00 |
+----+--------+-----+-----------+----------+



/*
display records with NAME values Khilan, Hardik and Muffy from the CUSTOMERS
*/

select * from customers where NAME in ('Khilan', 'Hardik', 'Muffy');

+----+--------+-----+---------+----------+
| ID | NAME   | AGE | ADDRESS | SALARY   |
+----+--------+-----+---------+----------+
|  2 | Khilan |  25 | Delhi   |  1500.00 |
|  5 | Hardik |  27 | Bhopal  |  8500.00 |
|  7 | Muffy  |  24 | Indore  | 10000.00 |
+----+--------+-----+---------+----------+



/*
displaying the records from CUSTOMERS table, 
where AGE is NOT equal to 25, 23 and 22.
*/

select  * from customers where AGE not in (25 , 23 , 22);

+----+--------+-----+-----------+----------+
| ID | NAME   | AGE | ADDRESS   | SALARY   |
+----+--------+-----+-----------+----------+
|  1 | Ramesh |  32 | Ahmedabad |  2000.00 |
|  5 | Hardik |  27 | Bhopal    |  8500.00 |
|  7 | Muffy  |  24 | Indore    | 10000.00 |
+----+--------+-----+-----------+----------+


/*
display all the records where the name starts with K
 and is at least 4 characters in length
*/

select * from customers where name like 'k___%';


+----+---------+-----+-----------+---------+
| ID | NAME    | AGE | ADDRESS   | SALARY  |
+----+---------+-----+-----------+---------+
|  2 | Khilan  |  25 | Delhi     | 1500.00 |
|  3 | Kaushik |  23 | Kota      | 2000.00 |
|  6 | Komal   |  22 | Hyderabad | 4500.00 |
+----+---------+-----+-----------+---------+


/*
  Get All the records from customer either  whose  age > 25 sal <= 4500
  and  either name should be komal , kaushik
*/

select * from customers where  (age>25 or salary <=4500 ) and 
(name='Komal' or name= 'Kaushik');


+----+---------+-----+-----------+---------+
| ID | NAME    | AGE | ADDRESS   | SALARY  |
+----+---------+-----+-----------+---------+
|  3 | Kaushik |  23 | Kota      | 2000.00 |
|  6 | Komal   |  22 | Hyderabad | 4500.00 |
+----+---------+-----+-----------+---------+



/*
fetch the top 4 records from the CUSTOMERS
*/

SELECT * FROM CUSTOMERS LIMIT 4;

+----+----------+-----+-----------+---------+
| ID | NAME     | AGE | ADDRESS   | SALARY  |
+----+----------+-----+-----------+---------+
|  1 | Ramesh   |  32 | Ahmedabad | 2000.00 |
|  2 | Khilan   |  25 | Delhi     | 1500.00 |
|  3 | Kaushik  |  23 | Kota      | 2000.00 |
|  4 | Chaitali |  25 | Mumbai    | 6500.00 |
+----+----------+-----+-----------+---------+


/*
retrieving the top 4 records of the CUSTOMERS table in a sorted order
based on salary
*/

SELECT * FROM CUSTOMERS  order by SALARY desc  LIMIT 4;

+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  7 | Muffy    |  24 | Indore    | 10000.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  6 | Komal    |  22 | Hyderabad |  4500.00 |
+----+----------+-----+-----------+----------+

/*
selects the first 40% of the records from the CUSTOMERS 
table sorted in the ascending order by their SALARY
*/

--> IN MYSQL this Query doesnt work

SELECT * FROM CUSTOMERS 
ORDER BY SALARY 
LIMIT (SELECT CEIL(COUNT(*) * 0.40) FROM CUSTOMERS);




/*
show the details of the first two customers
 whose name starts with K from the CUSTOMERS
*/

SELECT * FROM CUSTOMERS  WHERE NAME LIKE 'k%' LIMIT 2;

+----+---------+-----+---------+---------+
| ID | NAME    | AGE | ADDRESS | SALARY  |
+----+---------+-----+---------+---------+
|  2 | Khilan  |  25 | Delhi   | 1500.00 |
|  3 | Kaushik |  23 | Kota    | 2000.00 |
+----+---------+-----+---------+---------+



/*
lIST OF DISTINCT SALARIES FROM CUSTOMERS
*/

SELECT DISTINCT SALARY FROM CUSTOMERS; 


+----------+
| SALARY   |
+----------+
|  2000.00 |
|  1500.00 |
|  6500.00 |
|  8500.00 |
|  4500.00 |
| 10000.00 |
+----------+


/*
retrieving a list of all unique combinations of customer's age and salary
*/

SELECT DISTINCT AGE ,SALARY FROM CUSTOMERS;

+-----+----------+
| AGE | SALARY   |
+-----+----------+
|  32 |  2000.00 |
|  25 |  1500.00 |
|  23 |  2000.00 |
|  25 |  6500.00 |
|  27 |  8500.00 |
|  22 |  4500.00 |
|  24 | 10000.00 |
+-----+----------+



/*
we are retrieving the count of distinct age of the customers
*/

SELECT COUNT( DISTINCT AGE) FROM CUSTOMERS;
+----------------------+
| COUNT( DISTINCT AGE) |
+----------------------+
|                    6 |
+----------------------+



/*
retrieving all records from the CUSTOMERS table where the age of the customer 
is 25, and sorting them as per the descending order OF NAME
*/

SELECT * FROM CUSTOMERS WHERE AGE=25  ORDER BY NAME DESC;

+----+----------+-----+---------+---------+
| ID | NAME     | AGE | ADDRESS | SALARY  |
+----+----------+-----+---------+---------+
|  2 | Khilan   |  25 | Delhi   | 1500.00 |
|  4 | Chaitali |  25 | Mumbai  | 6500.00 |
+----+----------+-----+---------+---------+


## CUSTOM OR PREFFEREED ORDER ##

SELECT * FROM CUSTOMERS ORDER BY (
CASE ADDRESS
   WHEN 'MUMBAI' THEN 1
   WHEN 'DELHI' THEN 2
   WHEN 'HYDERABAD' THEN 3
   WHEN 'AHMEDABAD' THEN 4
   WHEN 'INDORE' THEN 5
   WHEN 'BHOPAL' THEN 6
   WHEN 'KOTA' THEN 7
   ELSE 100 END
);


+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  2 | Khilan   |  25 | Delhi     |  1500.00 |
|  6 | Komal    |  22 | Hyderabad |  4500.00 |
|  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  3 | Kaushik  |  23 | Kota      |  2000.00 |
+----+----------+-----+-----------+----------+



/*
finding the highest salary for each age IN ASCENDING ORDER
*/

SELECT MAX(SALARY) , AGE FROM CUSTOMERS GROUP BY AGE ORDER BY AGE;

+-------------+-----+
| MAX(SALARY) | AGE |
+-------------+-----+
|     4500.00 |  22 |
|     2000.00 |  23 |
|    10000.00 |  24 |
|     6500.00 |  25 |
|     8500.00 |  27 |
|     2000.00 |  32 |
+-------------+-----+


/*
grouping the records of the CUSTOMERS table 
based on the columns ADDRESS and AGE , GET THE AVERAGE SUM;
*/
SELECT ADDRESS , AVG(SALARY) , AGE  FROM  CUSTOMERS 
GROUP BY ADDRESS , AGE  ORDER BY AGE;

+-----------+--------------+-----+
| ADDRESS   | AVG(SALARY)  | AGE |
+-----------+--------------+-----+
| Hyderabad |  4500.000000 |  22 |
| Kota      |  2000.000000 |  23 |
| Indore    | 10000.000000 |  24 |
| Delhi     |  1500.000000 |  25 |
| Mumbai    |  6500.000000 |  25 |
| Bhopal    |  8500.000000 |  27 |
| Ahmedabad |  2000.000000 |  32 |
+-----------+--------------+-----+


/*
we are grouping the customers by their age and calculating 
the minimum salary for each group. Using the HAVING clause 
we are filtering the groups where the age is greater than 24
*/

SELECT AGE , MIN(SALARY) FROM CUSTOMERS GROUP BY AGE 
 HAVING AGE > 24  ORDER BY AGE;

+-----+-------------+
| AGE | MIN(SALARY) |
+-----+-------------+
|  25 |     1500.00 |
|  27 |     8500.00 |
|  32 |     2000.00 |
+-----+-------------+


/*
groups the records of the CUSTOMERS table based on the
 columns AGE and ADDRESS, filter the groups where the 
 SALARY value is less than 5000 and, 
 arranges the remaining groups in descending order 
 based the total salaries of each group.
 */
 
SELECT ADDRESS , AGE , SUM(SALARY) AS TOT_SAL FROM CUSTOMERS 
GROUP BY AGE , ADDRESS  HAVING TOT_SAL>=5000  ORDER BY TOT_SAL DESC;

+---------+-----+----------+
| ADDRESS | AGE | TOT_SAL  |
+---------+-----+----------+
| Indore  |  24 | 10000.00 |
| Bhopal  |  27 |  8500.00 |
| Mumbai  |  25 |  6500.00 |
+---------+-----+----------+


/*
we are retrieving all rows from the "CUSTOMERS" table 
where the age of the customer is equal to 25 or the salary
 is less than 4500 and the name is either Komal or Kaushik
 */
 
SELECT * FROM CUSTOMERS WHERE (AGE <=25  OR SALARY<=4500)  
AND (NAME = 'komal' OR NAME = 'Kaushik');
+----+---------+-----+-----------+---------+
| ID | NAME    | AGE | ADDRESS   | SALARY  |
+----+---------+-----+-----------+---------+
|  3 | Kaushik |  23 | Kota      | 2000.00 |
|  6 | Komal   |  22 | Hyderabad | 4500.00 |
+----+---------+-----+-----------+---------+




/*
  let us list out the details of all the CUSTOMERS whose 
  SALARY is greater than the SALARY of any customer whose AGE is 32
*/

SELECT * FROM CUSTOMERS 
WHERE  SALARY > ANY ( SELECT SALARY FROM CUSTOMERS WHERE AGE=32);

+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |
|  6 | Komal    |  22 | Hyderabad |  4500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+

```



```sql
/*
we are retrieving the details of all 
the customers whose age is equal to the age
 of any customer whose name starts with 'K'
*/

SELECT * FROM CUSTOMERS 
WHERE SALARY <> 
ALL (SELECT SALARY FROM CUSTOMERS WHERE AGE = 25);

+----+----------+-----+-----------+---------+
| ID | NAME     | AGE | ADDRESS   | SALARY  |
+----+----------+-----+-----------+---------+
|  2 | Khilan   |  25 | Delhi     | 1500.00 |
|  3 | Kaushik  |  23 | Kota      | 2000.00 |
|  4 | Chaitali |  25 | Mumbai    | 6500.00 |
|  6 | Komal    |  22 | Hyderabad | 4500.00 |
+----+----------+-----+-----------+---------+

/*
details of all the customers whose salary is not
 equal to the salary of any customer whose age is 25
 */

SELECT * FROM CUSTOMERS 
WHERE SALARY <> ALL ( SELECT SALARY FROM CUSTOMERS WHERE AGE=25);

+----+---------+-----+-----------+----------+
| ID | NAME    | AGE | ADDRESS   | SALARY   |
+----+---------+-----+-----------+----------+
|  1 | Ramesh  |  32 | Ahmedabad |  2000.00 |
|  3 | Kaushik |  23 | Kota      |  2000.00 |
|  5 | Hardik  |  27 | Bhopal    |  8500.00 |
|  6 | Komal   |  22 | Hyderabad |  4500.00 |
|  7 | Muffy   |  24 | Indore    | 10000.00 |
+----+---------+-----+-----------+----------+



/*
retrieving the lists of the customers 
with the price of the car greater than 2,000,000
*/

SELECT * FROM CUSTOMERS CU WHERE EXISTS
( SELECT PRICE FROM  CARS C  WHERE C.ID = CU.ID  AND PRICE > 2000000);

/*
+----+--------------+---------+
| ID | NAME         | PRICE   |
+----+--------------+---------+
|  2 | Maruti Swift |  450000 |
|  4 | VOLVO        | 2250000 |
|  7 | Toyota       | 2400000 |
+----+--------------+---------+
*/
+----+----------+-----+---------+----------+
| ID | NAME     | AGE | ADDRESS | SALARY   |
+----+----------+-----+---------+----------+
|  4 | Chaitali |  25 | Mumbai  |  6500.00 |
|  7 | Muffy    |  24 | Indore  | 10000.00 |
+----+----------+-----+---------+----------+


/*
gives the names of the customers who have not bought any car
*/

SELECT * FROM CUSTOMERS CU WHERE NOT EXISTS
(SELECT * FROM CARS  C WHERE  CU.ID = C.ID);

+----+---------+-----+-----------+---------+
| ID | NAME    | AGE | ADDRESS   | SALARY  |
+----+---------+-----+-----------+---------+
|  1 | Ramesh  |  32 | Ahmedabad | 2000.00 |
|  3 | Kaushik |  23 | Kota      | 2000.00 |
|  5 | Hardik  |  27 | Bhopal    | 8500.00 |
|  6 | Komal   |  22 | Hyderabad | 4500.00 |
+----+---------+-----+-----------+---------+


/*
If the AGE of the customer is greater than 30,
 it returns Gen X otherwise moves to the further 
 WHEN and THEN conditions. 
 If none of the conditions is matched 
 with the CUSTOMERS table, CASE returns the 'Gen Alpha'
*/

SELECT NAME , AGE , 
CASE 
WHEN AGE > 30 THEN 'GEN-X'
WHEN AGE > 24 THEN 'GEN-Y'
WHEN AGE > 22 THEN 'GNE-Z'
ELSE 'GEN-ALPHA'
END AS GENERATION FROM CUSTOMERS;

+----------+-----+------------+
| NAME     | AGE | GENERATION |
+----------+-----+------------+
| Ramesh   |  32 | GEN-X      |
| Khilan   |  25 | GEN-Y      |
| Kaushik  |  23 | GNE-Z      |
| Chaitali |  25 | GEN-Y      |
| Hardik   |  27 | GEN-Y      |
| Komal    |  22 | GEN-ALPHA  |
| Muffy    |  24 | GNE-Z      |
+----------+-----+------------+


/*
where we want to provide a 25% increment to each customer
 if the amount is less than 4500 from the CUSTOMERS
*/

SELECT * ,
CASE 
WHEN SALARY < 4500  THEN (SALARY + SALARY * 25/100)
END AS AFTER_INCREMENT FROM CUSTOMERS;


+----+----------+-----+-----------+----------+-----------------+
| ID | NAME     | AGE | ADDRESS   | SALARY   | AFTER_INCREMENT |
+----+----------+-----+-----------+----------+-----------------+
|  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |     2500.000000 |
|  2 | Khilan   |  25 | Delhi     |  1500.00 |     1875.000000 |
|  3 | Kaushik  |  23 | Kota      |  2000.00 |     2500.000000 |
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |            NULL |
|  5 | Hardik   |  27 | Bhopal    |  8500.00 |            NULL |
|  6 | Komal    |  22 | Hyderabad |  4500.00 |            NULL |
|  7 | Muffy    |  24 | Indore    | 10000.00 |            NULL |
+----+----------+-----+-----------+----------+-----------------+


/*
If the age of the customer is equal to '25',
 their salary will be updated to '17000'. 
 If the age is equal to '32', it will be updated to '25000'.
  For the customers with other ages, salaries will be updated to '12000'
*/


UPDATE CUSTOMERS SET  SALARY = 
CASE 
WHEN  AGE=25 THEN  17000
WHEN  AGE=32 THEN  25000
ELSE 12000
END;

 (OR) 
 
UPDATE CUSTOMERS SET  SALARY = 
CASE AGE
WHEN  25 THEN  17000
WHEN  32 THEN  25000
ELSE 12000
END;


+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  1 | Ramesh   |  32 | Ahmedabad | 25000.00 |
|  2 | Khilan   |  25 | Delhi     | 17000.00 |
|  3 | Kaushik  |  23 | Kota      | 12000.00 |
|  4 | Chaitali |  25 | Mumbai    | 17000.00 |
|  5 | Hardik   |  27 | Bhopal    | 12000.00 |
|  6 | Komal    |  22 | Hyderabad | 12000.00 |
|  7 | Muffy    |  24 | Indore    | 12000.00 |
+----+----------+-----+-----------+----------+


/*
we are retrieving all the customers whose salary
 is either ">2000" or "=2000". At the same time, 
 the customer must not be from "Bhopal"
*/

SELECT * FROM CUSTOMERS WHERE ADDRESS <> 'Bhopal' AND (SALARY >=2000);

+----+----------+-----+-----------+----------+
| ID | NAME     | AGE | ADDRESS   | SALARY   |
+----+----------+-----+-----------+----------+
|  1 | Ramesh   |  32 | Ahmedabad |  2000.00 |
|  3 | Kaushik  |  23 | Kota      |  2000.00 |
|  4 | Chaitali |  25 | Mumbai    |  6500.00 |
|  6 | Komal    |  22 | Hyderabad |  4500.00 |
|  7 | Muffy    |  24 | Indore    | 10000.00 |
+----+----------+-----+-----------+----------+



/*
returns the count of records have a blank field (NULL)
 in SALARY column of the CUSTOMERS table
*/

SELECT  COUNT(*) FROM CUSTOMERS WHERE SALARY IS NULL;
+----------+
| COUNT(*) |
+----------+
|        2 |
+----------+



```




## SET  OPERATORS
- UNION operator is used to combine data from multiple tables by **eliminating duplicate rows** (if any).
- all these tables must be union compatible.
    - The same number of columns selected with the same datatype.
    - These columns must also be in the same order.
    - They need not have same number of rows.
-  The difference between these two operators is that UNION only returns distinct rows while UNION ALL returns all the rows present in the tables.

```sql
## UNION ##


  CREATE TABLE CUSTOMERS (
     ID INT NOT NULL,
     NAME VARCHAR (20) NOT NULL,
     AGE INT NOT NULL,
     ADDRESS CHAR (25),
     SALARY DECIMAL (18, 2),       
     PRIMARY KEY (ID)
  );
  
  INSERT INTO CUSTOMERS VALUES
  (1, 'Ramesh', 32, 'Ahmedabad', 2000.00),
  (2, 'Khilan', 25, 'Delhi', 1500.00),
  (3, 'Kaushik', 23, 'Kota', 2000.00),
  (4, 'Chaitali', 25, 'Mumbai', 6500.00),
  (5, 'Hardik', 27, 'Bhopal', 8500.00),
  (6, 'Komal', 22, 'Hyderabad', 4500.00),
  (7, 'Muffy', 24, 'Indore', 10000.00);
  
  
  
  CREATE TABLE ORDERS (
     OID INT NOT NULL,
     DATE DATETIME NOT NULL,
     CUSTOMER_ID INT NOT NULL,
     AMOUNT INT NOT NULL,      
     PRIMARY KEY (OID)
  );
  
  INSERT INTO ORDERS VALUES
  (102, '2009-10-08 00:00:00', 3, 3000),
  (100, '2009-10-08 00:00:00', 3, 1500),
  (101, '2009-11-20 00:00:00', 2, 1560),
  (103, '2008-05-20 00:00:00', 4, 2060);
  
  
  /*
  COMBINE SALARY AND AMOUNT FROM BOTH THE TABLES
*/

SELECT SALARY FROM CUSTOMERS UNION SELECT AMOUNT FROM ORDERS;

+----------+
| SALARY   |
+----------+
|  2000.00 |
|  1500.00 |
|  6500.00 |
|  8500.00 |
|  4500.00 |
| 10000.00 |
|  1560.00 |
|  3000.00 |
|  2060.00 |
+----------+



/*
As the CUSTOMERS and ORDERS tables are not union-compatible individually,
let us first join these two tables into a bigger table using Left Join and
Right Join. The joined tables retrieved will have same number of columns 
with same datatypes, becoming union compatible. 
Now, these tables are combined using UNION
*/


SELECT  ID, NAME, SALARY,  AMOUNT, DATE FROM CUSTOMERS
LEFT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;

+----+----------+----------+--------+---------------------+
| ID | NAME     | SALARY   | AMOUNT | DATE                |
+----+----------+----------+--------+---------------------+
|  1 | Ramesh   |  2000.00 |   NULL | NULL                |
|  2 | Khilan   |  1500.00 |   1560 | 2009-11-20 00:00:00 |
|  3 | Kaushik  |  2000.00 |   3000 | 2009-10-08 00:00:00 |
|  3 | Kaushik  |  2000.00 |   1500 | 2009-10-08 00:00:00 |
|  4 | Chaitali |  6500.00 |   2060 | 2008-05-20 00:00:00 |
|  5 | Hardik   |  8500.00 |   NULL | NULL                |
|  6 | Komal    |  4500.00 |   NULL | NULL                |
|  7 | Muffy    | 10000.00 |   NULL | NULL                |
+----+----------+----------+--------+---------------------+

SELECT  ID, NAME, SALARY,  AMOUNT, DATE FROM CUSTOMERS
RIGHT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;

+------+----------+---------+--------+---------------------+
| ID   | NAME     | SALARY  | AMOUNT | DATE                |
+------+----------+---------+--------+---------------------+
|    3 | Kaushik  | 2000.00 |   1500 | 2009-10-08 00:00:00 |
|    2 | Khilan   | 1500.00 |   1560 | 2009-11-20 00:00:00 |
|    3 | Kaushik  | 2000.00 |   3000 | 2009-10-08 00:00:00 |
|    4 | Chaitali | 6500.00 |   2060 | 2008-05-20 00:00:00 |
+------+----------+---------+--------+---------------------+

SELECT  ID, NAME, AMOUNT, DATE FROM CUSTOMERS
LEFT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID
UNION
SELECT  ID, NAME, AMOUNT, DATE FROM CUSTOMERS
RIGHT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;


+------+----------+----------+--------+---------------------+
| ID   | NAME     | SALARY   | AMOUNT | DATE                |
+------+----------+----------+--------+---------------------+
|    1 | Ramesh   |  2000.00 |   NULL | NULL                |
|    2 | Khilan   |  1500.00 |   1560 | 2009-11-20 00:00:00 |
|    3 | Kaushik  |  2000.00 |   3000 | 2009-10-08 00:00:00 |
|    3 | Kaushik  |  2000.00 |   1500 | 2009-10-08 00:00:00 |
|    4 | Chaitali |  6500.00 |   2060 | 2008-05-20 00:00:00 |
|    5 | Hardik   |  8500.00 |   NULL | NULL                |
|    6 | Komal    |  4500.00 |   NULL | NULL                |
|    7 | Muffy    | 10000.00 |   NULL | NULL                |
+------+----------+----------+--------+---------------------+
 

CREATE TABLE STUDENTS(
   ID INT NOT NULL, 
   NAME VARCHAR(20) NOT NULL, 
   SUBJECT VARCHAR(20) NOT NULL, 
   AGE INT NOT NULL, 
   HOBBY VARCHAR(20) NOT NULL, 
   PRIMARY KEY(ID)
);

INSERT INTO STUDENTS VALUES
(1, 'Naina', 'Maths', 24, 'Cricket'),
(2, 'Varun', 'Physics', 26, 'Football'),
(3, 'Dev', 'Maths', 23, 'Cricket'),
(4, 'Priya', 'Physics', 25, 'Cricket'),
(5, 'Aditya', 'Chemistry', 21, 'Cricket'),
(6, 'Kalyan', 'Maths', 30, 'Football');

CREATE TABLE STUDENTS_HOBBY(
   ID INT NOT NULL, 
   NAME VARCHAR(20) NOT NULL, 
   HOBBY VARCHAR(20) NOT NULL, 
   AGE INT NOT NULL, 
   PRIMARY KEY(ID)
);

INSERT INTO STUDENTS_HOBBY VALUES
(1, 'Vijay', 'Cricket', 18),
(2, 'Varun', 'Football', 26),
(3, 'Surya', 'Cricket', 19),
(4, 'Karthik', 'Cricket', 25),
(5, 'Sunny', 'Football', 26),
(6, 'Dev', 'Cricket', 23);


SELECT SH.NAME, SH.AGE, SH.HOBBY FROM STUDENTS_HOBBY SH 
INNER JOIN STUDENTS S WHERE SH.ID = S.ID AND SH.AGE = S.AGE;


+---------+-----+----------+
| NAME    | AGE | HOBBY    |
+---------+-----+----------+
| Varun   |  26 | Football |
| Karthik |  25 | Cricket  |
+---------+-----+----------+

SELECT NAME , AGE , HOBBY FROM STUDENTS S WHERE S.ID  IN 
(SELECT ID FROM STUDENTS_HOBBY SH WHERE  S.AGE = SH.AGE);

+-------+-----+----------+
| NAME  | AGE | HOBBY    |
+-------+-----+----------+
| Varun |  26 | Football |
| Priya |  25 | Cricket  |
+-------+-----+----------+
```




## JOINS : 


### INNER JOINS :  
- An  [**﻿INNER JOIN**](https://www.tutorialspoint.com/sql/sql-inner-joins.htm)** ** is the default join which retrieves the intersection of two tables.
### OUTER JOINS : 
An Outer Join retrieves all the records in two tables even if there is no counterpart row of one table in another table.

- [﻿LEFT JOIN](https://www.tutorialspoint.com/sql/sql-left-joins.htm)  − returns all rows from the left table, even if there are no matches in the right table.
- [﻿RIGHT JOIN](https://www.tutorialspoint.com/sql/sql-right-joins.htm)  − returns all rows from the right table, even if there are no matches in the left table.
- [﻿FULL JOIN](https://www.tutorialspoint.com/sql/sql-full-joins.htm)  − returns rows when there is a match in one of the tables.
- [﻿SELF JOIN](https://www.tutorialspoint.com/sql/sql-self-joins.htm)  − is used to join a table to itself as if the table were two tables, temporarily renaming at least one table in the SQL statement.
- [﻿CROSS Join](https://www.tutorialspoint.com/sql/sql-cross-join.htm)  − returns the Cartesian product of the sets of records from the two or more joined tables.


![image.png](https://eraser.imgix.net/workspaces/RHvayis7uYQNdrSqkxAk/2TpPe0m2nPZODyVZctbl8Rh7kLL2/VChBImuD0cZOwvXE-pnxX.png?ixlib=js-3.7.0 "image.png")



```sql
CREATE TABLE CUSTOMERS (
   ID INT NOT NULL,
   NAME VARCHAR (20) NOT NULL,
   AGE INT NOT NULL,
   ADDRESS CHAR (25),
   SALARY DECIMAL (18, 2),       
   PRIMARY KEY (ID)
);

CREATE TABLE ORDERS (
   OID INT NOT NULL,
   DATE VARCHAR (20) NOT NULL,
   CUSTOMER_ID INT NOT NULL,
   AMOUNT DECIMAL (18, 2)
);

CREATE TABLE EMPLOYEE (
   EID INT NOT NULL,
   EMPLOYEE_NAME VARCHAR (30) NOT NULL,
   SALES_MADE DECIMAL (20)
);

INSERT INTO CUSTOMERS VALUES
(1, 'Ramesh', 32, 'Ahmedabad', 2000.00 ),
(2, 'Khilan', 25, 'Delhi', 1500.00 ),
(3, 'Kaushik', 23, 'Kota', 2000.00 ),
(4, 'Chaitali', 25, 'Mumbai', 6500.00 ),
(5, 'Hardik', 27, 'Bhopal', 8500.00 ),
(6, 'Komal', 22, 'Hyderabad', 4500.00 ),
(7, 'Muffy', 24, 'Indore', 10000.00 );


INSERT INTO ORDERS VALUES 
(102, '2009-10-08 00:00:00', 3, 3000.00),
(100, '2009-10-08 00:00:00', 3, 1500.00),
(101, '2009-11-20 00:00:00', 2, 1560.00),
(103, '2008-05-20 00:00:00', 4, 2060.00);


INSERT INTO EMPLOYEE VALUES
(102, 'SARIKA', 4500),
(100, 'ALEKHYA', 3623),
(101, 'REVATHI', 1291),
(103, 'VIVEK', 3426);

SELECT * FROM CUSTOMERS;
SELECT * FROM ORDERS;
SELECT * FROM EMPLOYEE;



SELECT * FROM CUSTOMERS C INNER JOIN ORDERS O ON C.ID = O.CUSTOMER_ID;

+----+----------+-----+---------+---------+-----+---------------------+-------------+---------+
| ID | NAME     | AGE | ADDRESS | SALARY  | OID | DATE                | CUSTOMER_ID | AMOUNT  |
+----+----------+-----+---------+---------+-----+---------------------+-------------+---------+
|  3 | Kaushik  |  23 | Kota    | 2000.00 | 102 | 2009-10-08 00:00:00 |           3 | 3000.00 |
|  3 | Kaushik  |  23 | Kota    | 2000.00 | 100 | 2009-10-08 00:00:00 |           3 | 1500.00 |
|  2 | Khilan   |  25 | Delhi   | 1500.00 | 101 | 2009-11-20 00:00:00 |           2 | 1560.00 |
|  4 | Chaitali |  25 | Mumbai  | 6500.00 | 103 | 2008-05-20 00:00:00 |           4 | 2060.00 |
+----+----------+-----+---------+---------+-----+---------------------+-------------+---------+

SELECT * FROM CUSTOMERS C
INNER JOIN ORDERS O ON C.ID = O.CUSTOMER_ID
INNER JOIN EMPLOYEE E ON O.OID = E.EID;


+----+----------+-----+---------+---------+-----+---------------------+-------------+---------+-----+---------------+------------+
| ID | NAME     | AGE | ADDRESS | SALARY  | OID | DATE                | CUSTOMER_ID | AMOUNT  | EID | EMPLOYEE_NAME | SALES_MADE |
+----+----------+-----+---------+---------+-----+---------------------+-------------+---------+-----+---------------+------------+
|  3 | Kaushik  |  23 | Kota    | 2000.00 | 102 | 2009-10-08 00:00:00 |           3 | 3000.00 | 102 | SARIKA        |       4500 |
|  3 | Kaushik  |  23 | Kota    | 2000.00 | 100 | 2009-10-08 00:00:00 |           3 | 1500.00 | 100 | ALEKHYA       |       3623 |
|  2 | Khilan   |  25 | Delhi   | 1500.00 | 101 | 2009-11-20 00:00:00 |           2 | 1560.00 | 101 | REVATHI       |       1291 |
|  4 | Chaitali |  25 | Mumbai  | 6500.00 | 103 | 2008-05-20 00:00:00 |           4 | 2060.00 | 103 | VIVEK         |       3426 |
+----+----------+-----+---------+---------+-----+---------------------+-------------+---------+-----+---------------+------------+


SELECT ID, NAME, DATE, AMOUNT FROM CUSTOMERS
INNER JOIN ORDERS
ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID
WHERE ORDERS.AMOUNT > 2000.00;

+----+----------+---------------------+---------+
| ID | NAME     | DATE                | AMOUNT  |
+----+----------+---------------------+---------+
|  3 | Kaushik  | 2009-10-08 00:00:00 | 3000.00 |
|  4 | Chaitali | 2008-05-20 00:00:00 | 2060.00 |
+----+----------+---------------------+---------+

```




```sql
SELECT ID, NAME, AMOUNT, OID ,  DATE
FROM CUSTOMERS
LEFT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;

+----+----------+---------+------+---------------------+
| ID | NAME     | AMOUNT  | OID  | DATE                |
+----+----------+---------+------+---------------------+
|  1 | Ramesh   |    NULL | NULL | NULL                |
|  2 | Khilan   | 1560.00 |  101 | 2009-11-20 00:00:00 |
|  3 | Kaushik  | 1500.00 |  100 | 2009-10-08 00:00:00 |
|  3 | Kaushik  | 3000.00 |  102 | 2009-10-08 00:00:00 |
|  4 | Chaitali | 2060.00 |  103 | 2008-05-20 00:00:00 |
|  5 | Hardik   |    NULL | NULL | NULL                |
|  6 | Komal    |    NULL | NULL | NULL                |
|  7 | Muffy    |    NULL | NULL | NULL                |
+----+----------+---------+------+---------------------+



SELECT ID, NAME, AMOUNT, OID ,  DATE , EID , EMPLOYEE_NAME
FROM CUSTOMERS
LEFT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID
LEFT JOIN EMPLOYEE ON EMPLOYEE.EID = ORDERS.OID;

+----+----------+---------+------+---------------------+------+---------------+
| ID | NAME     | AMOUNT  | OID  | DATE                | EID  | EMPLOYEE_NAME |
+----+----------+---------+------+---------------------+------+---------------+
|  1 | Ramesh   |    NULL | NULL | NULL                | NULL | NULL          |
|  2 | Khilan   | 1560.00 |  101 | 2009-11-20 00:00:00 |  101 | REVATHI       |
|  3 | Kaushik  | 1500.00 |  100 | 2009-10-08 00:00:00 |  100 | ALEKHYA       |
|  3 | Kaushik  | 3000.00 |  102 | 2009-10-08 00:00:00 |  102 | SARIKA        |
|  4 | Chaitali | 2060.00 |  103 | 2008-05-20 00:00:00 | NULL | NULL          |
|  5 | Hardik   |    NULL | NULL | NULL                | NULL | NULL          |
|  6 | Komal    |    NULL | NULL | NULL                | NULL | NULL          |
|  7 | Muffy    |    NULL | NULL | NULL                | NULL | NULL          |
+----+----------+---------+------+---------------------+------+---------------+



```


```sql
SELECT ID, NAME, SALARY , AMOUNT, DATE
FROM CUSTOMERS
RIGHT JOIN ORDERS
ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;


+------+----------+---------+---------+---------------------+
| ID   | NAME     | SALARY  | AMOUNT  | DATE                |
+------+----------+---------+---------+---------------------+
|    3 | Kaushik  | 2000.00 | 3000.00 | 2009-10-08 00:00:00 |
|    3 | Kaushik  | 2000.00 | 1500.00 | 2009-10-08 00:00:00 |
|    2 | Khilan   | 1500.00 | 1560.00 | 2009-11-20 00:00:00 |
|    4 | Chaitali | 6500.00 | 2060.00 | 2008-05-20 00:00:00 |
+------+----------+---------+---------+---------------------+

```


### FULL OUTER JOIN 
_MySQL does not support Full Outer Join. Instead, you can imitate its working by performing union operation between the result-sets obtained from Left Join and Right Join._



```sql
SELECT  ID, NAME, AMOUNT, DATE FROM CUSTOMERS
LEFT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID
UNION
SELECT  ID, NAME, AMOUNT, DATE FROM CUSTOMERS
RIGHT JOIN ORDERS ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;

+------+----------+---------+---------------------+
| ID   | NAME     | AMOUNT  | DATE                |
+------+----------+---------+---------------------+
|    1 | Ramesh   |    NULL | NULL                |
|    2 | Khilan   | 1560.00 | 2009-11-20 00:00:00 |
|    3 | Kaushik  | 1500.00 | 2009-10-08 00:00:00 |
|    3 | Kaushik  | 3000.00 | 2009-10-08 00:00:00 |
|    4 | Chaitali | 2060.00 | 2008-05-20 00:00:00 |
|    5 | Hardik   |    NULL | NULL                |
|    6 | Komal    |    NULL | NULL                |
|    7 | Muffy    |    NULL | NULL                |
+------+----------+---------+---------------------+
```








