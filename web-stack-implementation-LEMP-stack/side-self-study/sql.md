# Structured Query Language (SQL) Documentation

# Introduction

**SQL, known as Structured Query Language, serves as a robust and uniform tool crafted to engage with relational databases effectively. It enables individuals to carry out various operations such as data creation, retrieval, modification, and deletion within these databases. This documentation provides a foundational understanding of SQL syntax and explores the most frequently used commands.**


In SQL, the syntax and commands essentially refer to the same thing. Syntax outlines how to structure SQL statements, while commands are the specific instructions within those statements. 
Here's a breakdown:

1. **Syntax**: Defines the rules for writing SQL statements, such as using keywords like SELECT, INSERT, UPDATE, DELETE, etc., followed by specific parameters and clauses.

2. **Commands**: Are the actual instructions given to the database, following the syntax rules. For example, the SELECT command retrieves data, the INSERT command adds new records, the UPDATE command modifies existing records, and the DELETE command removes records.
Both syntax and commands are essential for effectively communicating with a database, ensuring that operations are performed accurately.**

## Basic SQL

### 1. SELECT

**SELECT** is likely the most frequently used SQL command. It's utilized nearly every time data is queried using SQL. This statement enables you to specify the data you wish your query to retrieve.

For instance, in the following code snippet, we're choosing a column named **"first_name"** from a table named **"clients"**.


```sql
SELECT first_name
FROM clients;
```
### 2. FROM

**FROM** specifies the table we are retrieving our data from:

```sql
SELECT first_name
FROM clients;
```

### 3. WHERE

**WHERE** filters your query to only return results that match a set of conditions. We can use this together with conditional operators like =, >, <, >=, <=, etc.


```sql
SELECT first_name
FROM clients
WHERE name = ‘Anna’;
```
### 4. GROUP BY

**GROUP BY** statement arranges rows with identical values into summarized rows, commonly employed alongside aggregate functions. 
For instance, in the following script below, **"Groups BY"** statement will group the rows based on the "first_name" column. This means that all rows with the same first_name will be grouped together, and the average age will be calculated for each group separately.

```sql
SELECT first_name, AVG(age)
FROM clients
GROUP BY first_name;
```
### 5. ORDER BY

**ORDER BY** command arranges the returned results in a specified order, typically ascending by default.

```sql
SELECT first_name
FROM clients
ORDER BY age;
```
## Conclusion
This documentation provides a good start to learn the basics of SQL and the commands people use most often. As we learn more and try out different types of queries, we'll get better at using SQL. This will help us manage and analyze data more effectively.