# Database Setup Guide

This guide will walk you through the process of setting up a PostgreSQL database on an EC2 instance. 

### 1. Connect to EC2 instance
Connect to your EC2 instance using ssh and the key-value pair for authentication:

```ssh -i key-value-pair-here.pem ubuntu@yourEC2IPpublicIpAddress```


### 2. Update and upgrade
Update the package list and upgrade the installed packages:

```sudo apt update && sudo apt upgrade -y```


### 3. Install PostgreSQL
Install PostgreSQL and its contrib package:

```sudo apt install postgresql postgresql-contrib -y```

### 4. Connect to the database with postgres user
Switch to the postgres user and enter the PostgreSQL prompt:
```sudo -i -u postgres```
```psql```

### 5. Create a new user
Create a new user with a password:
```CREATE USER ubuntu WITH PASSWORD 'ubuntu123';```

### 6. Double check user
Double check that the new user has been created:
```\du```

### 7. Connect ubuntu user to the PostgreSQL server
Connect to the PostgreSQL server using the newly created ubuntu user:
```psql -d postgres```

### 8. Create a new database
Create a new database:
```
psql -d postgres;
create database demo;
\c demo'
```

### 9. Create a database file in your local machine
Create a backup file of the database in your local machine:

```pg_dump dabasename > filename.sql```

### 10. Copy the file to the remote server
Copy the backup file to the remote server:
```scp -i [path to pem file] [path to demo.sql] username@[server-ip]:[directory to copy file to]```

### 11. Run the script
Restore the backup file to the new database:
```psql demo < /home/ubuntu/demo.sql```

Now your PostgreSQL database is up and running on localhost:5432 on your EC2 instance.

