---
title: Listing out all SQL objects that are not apart of the default setup
date: 2024-4-1 21:05:00 +0500
categories: [ MSSQL, Admin Report]
tags: [sql]
---
To generate a list of tables, views, stored procedures, and functions that are not in the list of default tables, functions, and views, you can use the following SQL query. First, create a list of the default objects, and then compare it with the objects in your database.

```sql
USE YourDatabaseName; -- Replace with your actual database name

-- Create a temporary table to store the list of default objects
CREATE TABLE #DefaultObjects (
    ObjectName NVARCHAR(128),
    ObjectType NVARCHAR(128)
);

-- Insert the default objects into the temporary table
INSERT INTO #DefaultObjects (ObjectName, ObjectType)
SELECT name, 'TABLE' AS ObjectType
FROM sys.tables
WHERE is_ms_shipped = 1
UNION ALL
SELECT name, 'VIEW' AS ObjectType
FROM sys.views
WHERE is_ms_shipped = 1
UNION ALL
SELECT name, 'PROCEDURE' AS ObjectType
FROM sys.procedures
WHERE is_ms_shipped = 1
UNION ALL
SELECT name, 'FUNCTION' AS ObjectType
FROM sys.objects
WHERE type IN ('FN', 'IF', 'TF', 'FS')
    AND is_ms_shipped = 1;

-- Generate a list of user-defined objects that are not in the default list
SELECT
    so.name AS ObjectName,
    CASE
        WHEN so.type = 'U' THEN 'TABLE'
        WHEN so.type = 'V' THEN 'VIEW'
        WHEN so.type = 'P' THEN 'PROCEDURE'
        WHEN so.type IN ('FN', 'IF', 'TF', 'FS') THEN 'FUNCTION'
        ELSE 'UNKNOWN'
    END AS ObjectType
FROM sys.objects so
LEFT JOIN #DefaultObjects do
    ON so.name = do.ObjectName
    AND
    CASE
        WHEN so.type = 'U' THEN 'TABLE'
        WHEN so.type = 'V' THEN 'VIEW'
        WHEN so.type = 'P' THEN 'PROCEDURE'
        WHEN so.type IN ('FN', 'IF', 'TF', 'FS') THEN 'FUNCTION'
        ELSE 'UNKNOWN'
    END = do.ObjectType
WHERE do.ObjectName IS NULL;

-- Clean up the temporary table
DROP TABLE #DefaultObjects;
```

Replace `YourDatabaseName` with the name of your database. This query will create a list of default tables, views, procedures, and functions, and then it will find the user-defined objects that are not in the default list. The result will include the names and types of the user-defined objects that are not part of the default set.