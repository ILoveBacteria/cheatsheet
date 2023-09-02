# MySQL Cheatsheet

## Table of Content
- [MySQL Cheatsheet](#mysql-cheatsheet)
  - [Table of Content](#table-of-content)
  - [Commands](#commands)
  - [Logical Operators](#logical-operators)
    - [ANY and ALL Operators](#any-and-all-operators)
  - [Case](#case)
  - [Functions](#functions)

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
- `ANY`
- `ALL`
- `EXISTS`

### ANY and ALL Operators

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