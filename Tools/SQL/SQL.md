# Structured Query Language (SQL)

## Introduction
Structured Query Language is a standard Database language which is used to create, maintain and retrieve the data from relational databases like MySQL, Oracle, SQL Server, PostGre, etc. As the name suggests, it is used when we have structured data (in the form of tables). All databases that are not relational (or do not use fixed structure tables to store data) and therefore do not use SQL, are called NoSQL databases. Examples of NoSQL are MongoDB, DynamoDB, Cassandra, etc

Structured Query Language is a standard Database language which is used to create, maintain and retrieve the relational database. **Relational database** means the data is stored as well as retrieved in the form of relations (tables).

- SQL is case insensitive. But it is a recommended practice to use keywords (like SELECT, UPDATE, CREATE, etc) in capital letters and use user defined things (liked table name, column name, etc) in small letters.
- There exist three different ways in writing comments.
    ```SQL
    ## Comments
    SELECT *
    FROM mytable; -- Comments
    /* 
    Comments1
    Comments2
    */
    ```
- SQL is the programming language for relational databases (explained below) like MySQL, Oracle, Sybase, SQL Server, Postgre, etc. Other non-relational databases (also called NoSQL) databases like MongoDB, DynamoDB, etc do not use SQL
- Although there is an ISO standard for SQL, most of the implementations slightly vary in syntax. So we may encounter queries that work in SQL Server but do not work in MySQL.

## Basics 
The queries to deal with relational database can be categories as:
- **Data Definition Language:** It is used to define the structure of the database. e.g; CREATE TABLE, ADD COLUMN, DROP COLUMN and so on.
- **Data Manipulation Language:** It is used to manipulate data in the relations. e.g.; INSERT, DELETE, UPDATE and so on.
- **Data Query Language:** It is used to extract the data from the relations. e.g.; SELECT

### Data Definition Language
Create and use an database:
```SQL
    CREATE DATABASE test;
    USE test;
```

Create a table:


### Data Query Language. 
A generic query to retrieve from a relational database is:
```SQL
    SELECT [DISTINCT] Attribute_List FROM R1,R2….RM
    [WHERE condition]
    [GROUP BY (Attributes)[HAVING condition]]
    [ORDER BY (Attributes)[ASC/DESC]];
```
Part of the query is compulsory if you want to retrieve from a relational database. The statements written inside [] are optional. 

**DISTINCT** is used to retrieve distinct values of an attribute or group of attribute. 

**AGGRATION FUNCTIONS:** Aggregation functions are used to perform mathematical operations on data values of a relation. All aggregation functions return only 1 row. Some of the common aggregation functions used in SQL are:
```SQL
    SELECT [AGGRATION FUNCTIONS] (attributename) FROM R1,R2...RM;
```
- **COUNT:** count the number of rows in a relation. e.g;
- **SUM:** add the values of an attribute in a relation. e.g;
- **AVERAGE:** give the average values of the tupples. It is also defined as sum divided by count values.
Syntax:`AVG(attributename)` OR Syntax:`SUM(attributename)/COUNT(attributename)`
- **MAXIMUM:** extract the maximum value among the set of tupples. Syntax: `MAX(attributename)`
- **MINIMUM:** extract the minimum value amongst the set of all the tupples. Syntax: `MIN(attributename)`

**GROUP BY** is used to group the tuples of a relation based on an attribute or group of attribute. It is always combined with aggregation function which is computed on group. e.g.;
```SQL
    SELECT ADDRESS, SUM(AGE) FROM STUDENT
    GROUP BY (ADDRESS);
```
In this query, SUM(AGE) will be computed but not for entire table but for each address. i.e.; sum of AGE for address DELHI and similarly for other address as well. 

If we try to execute the query given below, it will result in error because although we have computed SUM(AGE) for each address, there are more than 1 ROLL_NO for  each address we have grouped. So it can’t be displayed in result set. We need to use aggregate functions on columns after SELECT statement to make sense of the resulting set whenever we are using GROUP BY.
```SQL
    SELECT ROLL_NO, ADDRESS, SUM(AGE) FROM STUDENT
    GROUP BY (ADDRESS); 
```
**NOTE:** An attribute which is not a part of GROUP BY clause can’t be used for selection. Any attribute which is part of GROUP BY CLAUSE can be used for selection but it is not mandatory. But we could use attributes which are not a part of the GROUP BY clause in an aggregrate function.

## Reference
- [SQL tutorial at **geeksforgeeks**](https://www.geeksforgeeks.org/sql-tutorial/)

