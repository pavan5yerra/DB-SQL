## keys :


### UNIQUE KEY :
- It doesn't not allow duplicate keys in column
- The **unique **key is similar to the primary key, but it can accept **NULL values**, whereas the primary key does not.
- it accept only on NULL value
- it can also used as forgien key another table.
- table  can have more one unique keys


```sql
CREATE TABLE BUYERS (
   ID INT NOT NULL UNIQUE KEY,
   NAME VARCHAR(20) NOT NULL UNIQUE KEY,
   AGE INT NOT NULL,
   ADDRESS CHAR (25),
   SALARY DECIMAL (18, 2)
);

--> Verification
INSERT INTO BUYERS VALUES 
(1, 'Ramesh', 32, 'Ahmedabad', 2000.00 ),
(1, 'Rajesh', 25, 'Delhi', 1500.00 );

ERROR 1062 (23000): Duplicate entry '1' for key 'customers.ID'
```


### PRIMARY KEY : 
- Even though table should have one primary key , it can have multiple primary keys , it will be considered as composite key
- Column contrained with primary key can have unique values only
- cannot accept null values unlike UNIQUE KEY
- A primary key length cannot be more than 900 bytes.


```sql
CREATE TABLE CUSTOMERS (
   ID INT NOT NULL,
   NAME VARCHAR (20) NOT NULL,
   AGE INT NOT NULL,
   ADDRESS CHAR (25),
   SALARY DECIMAL (18, 2),       
   PRIMARY KEY (ID)
);

## COMPOSITE KEY ##

CREATE TABLE CUSTOMERS(
   ID INT NOT NULL,
   NAME VARCHAR (20) NOT NULL,
   AGE INT NOT NULL,
   ADDRESS CHAR (25),
   SALARY DECIMAL (18, 2),       
   CONSTRAINT ck_customers 
   PRIMARY KEY (ID, NAME)
);
```




### FOREIGN KEY : 
- _The foreign key can reference the unique fields of any table in the database. _
- _The table that has the primary key is known as the parent table and the key with the foreign key is known as the child table._
- It is used to reduce the redundancy in the tables
- It helps normalize the tables
```sql
CREATE TABLE ORDERS (
   ID INT NOT NULL,
   DATE DATETIME, 
   CUSTOMER_ID INT,
   CONSTRAINT FK_CUSTOMER 
 FOREIGN KEY(CUSTOMER_ID) 
REFERENCES CUSTOMERS(ID),
   AMOUNT DECIMAL,
   PRIMARY KEY (ID)
);
```




### ALTERNATE KEYS:
- These are candidate keys that are not currently selected as primary keys

![image.png](https://eraser.imgix.net/workspaces/RHvayis7uYQNdrSqkxAk/2TpPe0m2nPZODyVZctbl8Rh7kLL2/9ECXQyaKT9pu0sWt2HzTV.png?ixlib=js-3.7.0 "image.png")



## Indexes :
- These are special lookup tables , which will speed the process of data retrival


```sql
CREATE INDEX index_name ON table_name;
```
### Types of Indexes :
- Unique Index
    - These are not only for performance  , it also allows duplicate columns in table.
    - It is automatically created by PRIMARY and UNIQUE constraints
```sql
CREATE UNIQUE INDEX index_name
on table_name (column_name);
```
- Single-Column Index
    - A single-column index is created only on one table column.
```sql
CREATE INDEX index_name
ON table_name (column_name);
```
- Composite Index
    - A composite index is an index that can be created on two or more columns of a table.
```sql
CREATE INDEX index_name
on table_name (column1, column2);
```
- Implicit Index
    - Implicit indexes are indexes that are automatically created by the database
    - indexes are automatically created when primary key and unique constraints are created 
