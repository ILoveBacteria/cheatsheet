# MySQL Cheatsheet

## Table of Content
- [MySQL Cheatsheet](#mysql-cheatsheet)
  - [Table of Content](#table-of-content)
  - [Commands](#commands)
  - [Logical Operators](#logical-operators)
  - [Case](#case)
  - [Functions](#functions)
  - [Constraints](#constraints)
    - [Unique](#unique)
    - [Primary Key](#primary-key)
    - [Foreign Key](#foreign-key)
    - [Check](#check)
    - [Default](#default)
    - [Index](#index)
    - [Auto Increment](#auto-increment)
  - [Data Types](#data-types)
    - [Date](#date)
  - [Handwrite Notes](#handwrite-notes)

## Commands

- `SHOW DATABASES`
- `use mysql`: select a database
- `SHOW TABLES`

- `SELECT host,user FROM user`
- `DESC user`: more information on the user table.
- `select user()` Current user

- `ALTER USER 'user'@'hostname' IDENTIFIED BY 'newPass'`: Change password
- `GRANT ALL PRIVILEGES ON yourDB.* TO user1@'133.155.44.103' IDENTIFIED BY 'password1'`
- `SHOW GRANTS FOR 'username'@'host'`
- `REVOKE type_of_permission ON database_name.table_name FROM 'username'@'host';`: Remove a grant
- `REVOKE ALL PRIVILEGES ON database_name.table_name FROM 'username'@'host';`: Remove all grant

- `DROP USER 'jeffrey'@'localhost'`: Delete a user
- `CREATE USER 'sammy'@'localhost' IDENTIFIED BY 'password'`

`FLUSH PRIVILEGES` Many guides suggest running the FLUSH PRIVILEGES command immediately after a CREATE USER or GRANT statement in order to reload the grant tables to ensure that the new privileges are put into effect:

## Logical Operators

- `AND`
- `OR`
- `NOT`
- `BETWEEN`
- `IN`
- `LIKE`
- `ALL`
- `EXISTS`
- `ANY`
    ```sql
    SELECT column_name(s)
    FROM table_name
    WHERE column_name operator ANY
      (SELECT column_name
      FROM table_name
      WHERE condition);
    ```

## Case

```sql
SELECT OrderID, Quantity,
CASE
    WHEN Quantity > 30 THEN 'The quantity is greater than 30'
    WHEN Quantity = 30 THEN 'The quantity is 30'
    ELSE 'The quantity is under 30'
END AS QuantityText
FROM OrderDetails;
```

## Functions

- `CONCAT()`: Concatenate strings
- `CONCAT_WS()`: Concatenate with separator
- `CHAR_LENGTH()`: Return the number of characters in a string
- `CONVERT()`: Convert a value into a specified datatype [link](https://www.w3schools.com/sql/func_mysql_convert.asp)
- `ST_X()`: Return X coordinate of point
- `ST_Y()`: Return Y coordinate of point
- `ST_DISTANCE()`: Return distance between two points
- `ST_DISTANCE_SPHERE()`: Return distance between two points on a sphere in meters
- `IFNULL()`: If expr1 is not NULL, IFNULL() returns expr1; otherwise it returns expr2.
    ```sql
    SELECT ProductName, UnitPrice * (UnitsInStock + IFNULL(UnitsOnOrder, 0))
    FROM Products;
    ```
- `COALESCE()`: Return the first non-NULL argument
    ```sql
    SELECT ProductName, UnitPrice * (UnitsInStock + COALESCE(UnitsOnOrder, 0))
    FROM Products;
    ```

## Constraints

- `NOT NULL` - Ensures that a column cannot have a NULL value
- `UNIQUE` - Ensures that all values in a column are different
- `PRIMARY KEY` - A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table
- `FOREIGN KEY` - Prevents actions that would destroy links between tables
- `CHECK` - Ensures that the values in a column satisfies a specific condition
- `DEFAULT` - Sets a default value for a column if no value is specified
- `CREATE INDEX` - Used to create and retrieve data from the database very quickly

### Unique

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    UNIQUE (ID)
    -- OR with name
    CONSTRAINT UC_Person UNIQUE (ID,LastName)
);
```

```sql
-- Add UNIQUE
ALTER TABLE Persons
ADD UNIQUE (ID);
-- OR
ADD CONSTRAINT UC_Person UNIQUE (ID,LastName);

-- Drop UNIQUE
ALTER TABLE Persons
DROP INDEX UC_Person;
```

### Primary Key

```sql
-- Drop a primary key
ALTER TABLE Persons
DROP PRIMARY KEY;
```

### Foreign Key

```sql
CREATE TABLE Orders (
    OrderID int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
    -- OR
    CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID)
    REFERENCES Persons(PersonID)
);
```

### Check

```sql
CREATE TABLE Persons (
    Age int,
    CHECK (Age>=18)
);
```

### Default

```sql
-- Create table
City varchar(255) DEFAULT 'something'
OrderDate date DEFAULT CURRENT_DATE()
-- Alter add
ALTER TABLE Persons
ALTER City SET DEFAULT 'something';
-- Alter drop
ALTER TABLE Persons
ALTER City DROP DEFAULT;
```

### Index

```sql
-- Duplicate values are allowed
CREATE INDEX index_name
ON table_name (column1, column2, ...);
-- Unique values
CREATE UNIQUE INDEX index_name
```

### Auto Increment

```sql
-- Create table
Personid int NOT NULL AUTO_INCREMENT,
-- sequence start with another value
ALTER TABLE Persons AUTO_INCREMENT=100;
```

## Data Types

### Date

- `DATE` - format YYYY-MM-DD
- `DATETIME` - format: YYYY-MM-DD HH:MI:SS
- `TIMESTAMP` - format: YYYY-MM-DD HH:MI:SS
- `YEAR` - format YYYY or YY

## Handwrite Notes

```sql
-- Add column to a specific position
ALTER TABLE locations
ADD region_id INT 
AFTER state_province;
-- Add to first
ALTER TABLE locations
ADD region_id INT FIRST;
```