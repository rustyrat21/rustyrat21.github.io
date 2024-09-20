---
title: Pull the dependencies from all Reports built in Report Manager
date: 2024-09-19 20:40:00 +0500
categories: [MSSQL]
tags: [sql,ssrs,report manager,dependencies]
---
In SQL Server 2014, you can query the ReportServer database to pull the dependencies from all reports built in Report Manager. Here's a basic query that retrieves the dependencies for all reports:

```sql
SELECT 
    C.Name AS ReportName,
    D.Path AS DataSourcePath,
    D.Name AS DataSourceName,
    D.Extension AS DataSourceExtension
FROM 
    Catalog AS C
JOIN 
    DataSource AS D ON C.ItemID = D.ItemID
WHERE 
    C.Type = 2; -- Type 2 represents reports in the Catalog table
```

This query retrieves the report name along with the associated data source path, name, and extension. You can modify the query to include additional information as needed.

Please note that you'll need appropriate permissions to access the ReportServer database and run this query.

This query will not pull dependencies where queries are placed within the Report, only when the dependencies are setup as Data Sources.