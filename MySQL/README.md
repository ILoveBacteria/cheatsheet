# MySQL Cheatsheet

## Table of Content
- [MySQL Cheatsheet](#mysql-cheatsheet)
  - [Table of Content](#table-of-content)
  - [Commands](#commands)

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