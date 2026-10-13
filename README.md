Amazon RDS MariaDB Setup Guide

This guide explains how to connect to an Amazon RDS MariaDB database from an EC2 instance.

1. Create the Database Using Amazon RDS

   Open the AWS Management Console and go to Amazon RDS.

   Create a MariaDB database.

   Note the database endpoint, port, database name, and master username.

   Keep the database in the same VPC as your EC2 instance.

Important: Amazon RDS manages the database server. You do not need to install mariadb-server on EC2.

2. Install the MySQL Client on EC2

Run these commands on your Ubuntu EC2 instance:

sudo apt update

sudo apt install mysql-client -y

Verify the installation:

mysql --version

3. Connect to Amazon RDS

Use the endpoint shown under RDS → Databases → your database → Connectivity & security.

mysql -h <RDS_ENDPOINT> -P 3306 -u <DB_USERNAME> -p

Replace <RDS_ENDPOINT> with your RDS endpoint and <DB_USERNAME> with your database username. 
Enter the root password when prompted.

Create a new database and user:

```sql
CREATE DATABASE student_db;
GRANT ALL PRIVILEGES ON springbackend.* TO 'username'@'localhost' IDENTIFIED BY 'your_password';
```
Replace username and your_password with your desired username and password.



## 4. You will need Database Credentials to Connect Backend with Database
1. DB_HOST
2. DB_USER
3. DB_PASS
4. DB_PORT

5. DB_NAME

```sh
USE student_db;
```
```sh
CREATE TABLE `students` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `email` varchar(255) DEFAULT NULL,
  `course` varchar(255) DEFAULT NULL,
  `student_class` varchar(255) DEFAULT NULL,
  `percentage` double DEFAULT NULL,
  `branch` varchar(255) DEFAULT NULL,
  `mobile_number` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=80 DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
```
EXIT FROM DATABASE
```sql

EXIT;
```



## Database Connection URL Reminder

For Spring Boot with Amazon RDS MariaDB, AWS provides the endpoint, but you must add the JDBC prefix yourself.

Format:
jdbc:mariadb://<RDS_ENDPOINT>:3306/<DATABASE_NAME>

- `jdbc:mariadb://` — add this prefix yourself.
- `<RDS_ENDPOINT>` — copy from RDS → Connectivity & security.
- `3306` — database port.
- `<DATABASE_NAME>` — the database you created.

Use environment variables to keep credentials out of GitHub:

spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
