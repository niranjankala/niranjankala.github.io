---
title: "How to Fix ORA-01489: Result of String Concatenation is Too Long with LISTAGG Function"
published: true
description: "How to Fix ORA-01489: Result of String Concatenation is Too Long with LISTAGG Function in Oracle"
tags: PostgreSQL
cover_image: 
canonical_url: https://niranjankala.in/post/how-to-fix-ora-01489-result-of-string-concatenation-is-too-long-with-listagg-function
layout: post
---
    

## Introduction
Oracle Database is a powerful relational database management system offering developers and database administrators various functionalities. One of these powerful features is the `LISTAGG` function, which allows you to aggregate rows of data into a single concatenated string. However, when using the `LISTAGG` function with large datasets, you may encounter an error message like "ORA-01489: result of string concatenation is too long." In this article, we will explore the causes of this error and discuss various methods to prevent and handle it effectively.

## Understanding the ORA-01489 Error

The `ORA-01489` error occurs when the result of concatenating strings with the `LISTAGG` function exceeds the maximum allowed length for a string in Oracle, which is 4000 characters for non-CLOB data types in most environments. When the concatenated string surpasses this limit, the error is triggered, and the query fails to execute.

## Solutions to Prevent ORA-01489 Error

### 1. Truncate the Result with `ON OVERFLOW TRUNCATE`:

Starting from Oracle 19c, you can use the `ON OVERFLOW TRUNCATE` clause with the `LISTAGG` function. This option truncates the concatenated string to fit within the maximum string length allowed.

```sql
SELECT 
    LISTAGG(column, delimiter) WITHIN GROUP (ORDER BY column) ON OVERFLOW TRUNCATE AS concatenated_result
FROM 
    table_name;
```

Please be careful when using this approach since it may lead to data loss if the truncated portion contains essential information.

### 2. Filter Data to Reduce Concatenation Size:

If the error occurs due to a large number of rows being concatenated, consider filtering the data before using the `LISTAGG` function. Reducing the number of rows in the result set can help keep the concatenated string within the allowed length.

```sql
SELECT 
    LISTAGG(column, delimiter) WITHIN GROUP (ORDER BY column) AS concatenated_result
FROM 
    table_name
WHERE 
    /* Add appropriate conditions to filter data */
    condition;
```

### 3. Use CLOB Data Type for Concatenation:

If the concatenated result is expected to exceed the maximum allowed length for a non-CLOB data type, you can use the CLOB data type to store the result. CLOB can handle much larger strings, making it suitable for situations requiring extensive concatenation.

```sql
SELECT 
    TO_CLOB(LISTAGG(column, delimiter) WITHIN GROUP (ORDER BY column)) AS concatenated_clob
FROM 
    table_name;
```

Please note that CLOB data type has its limitations, and you should ensure it aligns with your database configuration.

### 4. Partition the Data:

If possible, partition the data and perform concatenation on smaller subsets. You can then combine the results of each subset to get the final concatenated string.

```sql
SELECT 
    LISTAGG(column, delimiter) WITHIN GROUP (ORDER BY column) AS concatenated_result
FROM 
    (
        /* Subquery 1: Concatenate first partition */
        SELECT column FROM table_name PARTITION (partition_name_1)
        UNION ALL
        /* Subquery 2: Concatenate second partition */
        SELECT column FROM table_name PARTITION (partition_name_2)
        /* Add more subqueries as needed for other partitions */
    );
```

### 5. Use XMLAGG and LISTAGG Combination:

Another approach to overcome the ORA-01489 error is using `XMLAGG` in combination with `LISTAGG`. The `XMLAGG` function helps avoid the string length limitation by aggregating data into XML elements before concatenation.

```sql
SELECT 
    RTRIM(
        XMLAGG(XMLELEMENT(e, column || delimiter) ORDER BY column).EXTRACT('//text()'),
        delimiter
    ) AS concatenated_result
FROM 
    table_name;
```

### 6. Limit the Concatenation Length:

If you don't want to truncate the result or use CLOB, you can limit the maximum length of the concatenated string to a specific value using the `SUBSTR` function.

```sql
SELECT 
    SUBSTR(LISTAGG(column, delimiter) WITHIN GROUP (ORDER BY column), 1, max_length) AS concatenated_result
FROM 
    table_name;
```

In this example, `max_length` should be the maximum allowed length for the concatenated string.

## Conclusion

The `LISTAGG` function in Oracle Database is valuable for aggregating data into a single concatenated string. However, if the result exceeds the maximum string length allowed, the ORA-01489 error may occur when dealing with large datasets. Applying one or a combination of the solutions provided in this article can effectively handle and prevent the ORA-01489 error, allowing you to manage and manipulate large datasets without encountering string concatenation length limitations. You can choose the solution that best fits your specific use case and database configuration to ensure efficient and error-free data aggregation.


