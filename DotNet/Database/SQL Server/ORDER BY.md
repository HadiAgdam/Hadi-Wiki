


## Multiple `ORDER BY` columns

Your query:

```sql
SELECT
    Colors.Name AS Color,
    Sizes.Name AS Size
FROM Colors
CROSS JOIN Sizes
ORDER BY Color DESC, Size ASC;
```

This means:

1. Sort by `Color` descending.
2. **When two rows have the same Color**, sort those rows by `Size` ascending.