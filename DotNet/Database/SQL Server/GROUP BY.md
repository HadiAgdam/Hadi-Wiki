

```sql
SELECT Name FROM Users GROUP BY Name;
```

Result:
```
Ali
Hadi
Reza
```

---

```sql
SELECT Name, COUNT(*)
FROM Users
GROUP BY Name;
```

Result:
```
Name  (No column name)

Ali     2
Hadi    3
Reza    1
```


---

`GROUP BY` is usually used together with aggregate functions such as:
```sql
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

### Using `AS`:

```sql
SELECT [Name] AS [N], COUNT(*) AS [Count] FROM [Users] GROUP BY Name;
```

Result:
```
N    Count

Ali    2
Hadi   3
Reza   1

```

---

### Using `HAVING`:

if You wanted to find names that appear more than once:

```sql
SELECT Name, COUNT(*) As [Count]
FROM Users
GROUP BY Name
HAVING COUNT(*) > 1; -- there is more the 1 row inside group
```


### `HAVING` vs `WHERE`:


```sql
select FirstName as [Name], count(*) from Users where UserGroupId = 3 group by FirstName having count(*) > 1;
```

`WHERE` happens before the grouping, while `HAVING` works on the groups.


---

### Group by two fields


```sql
SELECT FirstName, LastName, COUNT(*) AS UserCount
FROM Users
GROUP BY FirstName, LastName
HAVING COUNT(*) > 1;
```