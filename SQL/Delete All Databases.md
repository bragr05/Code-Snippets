![Warning](https://img.shields.io/badge/Warning-Important-orange)


To delete all databases on a server except for  `master`, `model,` `msdb`, `tempdb` execute the following code

```sql
DECLARE @DatabaseName NVARCHAR(255)
DECLARE @sql NVARCHAR(500)

DECLARE db_cursor CURSOR FOR
SELECT name
FROM sys.databases
WHERE name NOT IN ('master', 'model', 'msdb', 'tempdb')

OPEN db_cursor
FETCH NEXT FROM db_cursor INTO @DatabaseName

WHILE @@FETCH_STATUS = 0
BEGIN
    SET @sql = 'ALTER DATABASE [' + @DatabaseName + '] SET SINGLE_USER WITH ROLLBACK IMMEDIATE'
    EXEC sp_executesql @sql

    SET @sql = 'DROP DATABASE [' + @DatabaseName + ']'
    EXEC sp_executesql @sql

    FETCH NEXT FROM db_cursor INTO @DatabaseName
END

CLOSE db_cursor
DEALLOCATE db_cursor

```

