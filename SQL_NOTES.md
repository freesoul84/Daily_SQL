##### To check list of databases present in SQL Server
```
SELECT name FROM master.dbo.sysdatabases
```

##### Create database in SQL
```
CREATE DATABASE database_name
```
here it doesn't mater if the name is small or capital. it is incase sensitive


#### matching of % in a string
```
SELECT * FROM TABLENAME WHERE COLUMNNAME LIKE '%[%]%'
```
here we need to include that special character in bracket


# IMPORTANT POINTS

* To conatenate string in T-SQL we can use + sign :
  for example. 'ALKESHA' + ',' + 'EKTA'
* To trim the data or white space in left we use LTRIM and to trip it from right we can use RTRIM and to trim overall we can use TRIM
* TOP keyword we need to use in SELECT. 
  for example. SELECT TOP N FROM TABLE_NAME