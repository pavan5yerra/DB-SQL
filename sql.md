

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

