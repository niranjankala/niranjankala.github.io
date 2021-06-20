---
title: "SQLite- Check If Table Exists"
published: true
description: "SQLite- Check If Table Exists"
tags: sqlite
cover_image: 
canonical_url: http://niranjankala.in/post/sqlite-check-if-table-exists
layout: post
---
    
### Introduction

In this article, you will learn to check if a table exists in the SQLite database or not.


### How to check if a table exists in the database


You execute the below command to check whether a table exists or not in the SQLite database:

```sql
SELECT count(*) FROM sqlite_master WHERE type='table' AND name='tableName';
```

It will return value either 0 or greater than 0. If the table doesn't exist then the result will be 0 otherwise 1.

You can use this query in any programming language to the existence of the table in the database. For an example below .NET code block will check the existence of the table in the sqlite database:
```csharp 
string sqliteDBFile = string.Format("{0}\\{1}", "D:\\Logs", "Log.db");
string tableName = "LogTable";
string columnName = "CreatedOn";
if (!File.Exists(sqliteLogDBFile))
{
    using (SQLiteConnection con = new SQLiteConnection(string.Format("data source={0}", sqliteLogDBFile)))
    {
       if (CheckIfTableExists(con, Constants.SQLiteLogTableName))
       {
            if (!CheckIfColumnExists(con, tableName, columnName))
            {
                        
            }
        }
    }
}

private bool CheckIfTableExists(SQLiteConnection conn, string tableName)
{
    if (conn.State == System.Data.ConnectionState.Closed)
        conn.Open();

    using (SQLiteCommand cmd = new SQLiteCommand(conn))
    {
        cmd.CommandText = $"SELECT count(*) FROM sqlite_master WHERE type='table' AND name='{tableName}';";
        object result = cmd.ExecuteScalar();
        int resultCount = Convert.ToInt32(result);
        if (resultCount > 0)
            return true;

    }
    return false;
}
```

You can also check that that if a column exists in the table or not using the below line of code which using the table metadata and then check the column name for the match.

```csharp
private bool CheckIfColumnExists(SQLiteConnection conn, string tableName, string columnName)
{
    if (conn.State == System.Data.ConnectionState.Closed)
        conn.Open();

    using (SQLiteCommand cmd = new SQLiteCommand(conn))
    {
        cmd.CommandText = string.Format("PRAGMA table_info({0})", tableName);

        var reader = cmd.ExecuteReader();
        int nameIndex = reader.GetOrdinal("Name");
        while (reader.Read())
        {
            if (reader.GetString(nameIndex).Equals(columnName))
            {
                return true;
            }
        }
    }
    return false;
}
```

### Checking a table exists or not before creating a new table

Checking a table before creating or dropping a table is the common use case. To run the query without failing it with a fail-safe check. In SQLite, you can sure that table should be created in the database if it does not exist using the below query:

```sql
CREATE TABLE IF NOT EXISTS <table_name> (column1_name <datatype>,....)
```




### Conclusion
You have learned that how can check the existence of the database base objects in an SQLite database.
