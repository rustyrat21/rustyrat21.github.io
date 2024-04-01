---
title: List all Reports from Report Manager
date: 2024-04-01 21:05:00 +0500
categories: [ MSSQL, Report Manager,Admin Report]
tags: [sql,srss]
---

In SQL Server 2014, you can use the following query to pull down details of all reports in Report Manager from the ReportServer database:

```sql
USE ReportServer; -- Assuming 'ReportServer' is the name of your ReportServer database

SELECT
    C.Name AS ReportName,
    C.Path AS ReportPath,
    C.Description AS ReportDescription,
    C.CreationDate AS ReportCreationDate,
    C.ModifiedDate AS ReportModifiedDate,
    U.UserName AS CreatedBy,
    U2.UserName AS ModifiedBy
FROM
    Catalog AS C
JOIN
    Users AS U ON C.CreatedByID = U.UserID
JOIN
    Users AS U2 ON C.ModifiedByID = U2.UserID
WHERE
    C.Type = 2; -- 2 indicates report type
```

Please replace `'ReportServer'` with the actual name of your ReportServer database. This query joins the `Catalog` table with the `Users` table twice to retrieve details about each report's name, path, description, creation date, modification date, creator, and modifier. The `Type = 2` condition filters the results to only include reports.

Note that this query assumes that you have permission to access the `ReportServer` database and its tables. Make sure to run this query in the context of the `ReportServer` database. Also, keep in mind that the structure of the database might have changed in newer versions of SQL Server or Reporting Services.