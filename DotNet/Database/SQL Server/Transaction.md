

```sql
BEGIN TRANSACTION;

UPDATE Accounts
SET Balance = Balance - 30
WHERE Id = 1;

UPDATE Accounts
SET Balance = Balance + 30
WHERE Id = 2;

COMMIT TRANSACTION;
```

```
BEGIN
  ↓
Do operations
  ↓
Everything successful?
  ↓
COMMIT
```



### Rollback

```sql
BEGIN TRANSACTION;

UPDATE Accounts
SET Balance = Balance - 30
WHERE Id = 1;

-- Something goes wrong here

UPDATE Accounts
SET Balance = Balance + 30
WHERE Id = 999;

ROLLBACK TRANSACTION;
```


```
BEGIN
 ↓
UPDATE #1
 ↓
UPDATE #2 ❌
 ↓
ROLLBACK
 ↓
Database returns to previous state
```


### `SET XACT_ABORT ON`

```sql
SET XACT_ABORT ON;
```

>If a runtime error occurs, automatically terminate and roll back the current transaction.


```sql
SET XACT_ABORT ON;
BEGIN TRANSACTION;

UPDATE Users
SET Name = 'Hadi'
WHERE Id = 10;

-- Imagine this causes a runtime error
  
INSERT INTO SomeTable (...)
VALUES (...);

COMMIT TRANSACTION
```


### Try catch

```sql
SET XACT_ABORT ON;

BEGIN TRY

    BEGIN TRANSACTION;

    UPDATE ...
    INSERT ...
    DELETE ...

    COMMIT TRANSACTION;

END TRY
BEGIN CATCH

    ROLLBACK TRANSACTION;

    THROW;

END CATCH;
```


### What is `@@TRANCOUNT`?

```sql
@@TRANCOUNT
```
tells you how many active transactions exist for your current SQL connection.


```sql
IF @@TRANCOUNT > 0
    ROLLBACK TRANSACTION;
```

means:
> Only rollback if there is actually a transaction to roll back.





```sql
SET XACT_ABORT ON;

BEGIN TRY

    BEGIN TRANSACTION;

    -- Operation 1

    -- Operation 2

    -- Operation 3

    COMMIT TRANSACTION;

END TRY
BEGIN CATCH

    IF @@TRANCOUNT > 0
        ROLLBACK TRANSACTION;

    THROW;

END CATCH;
```



### One very important thing: transaction ≠ database backup

A transaction doesn't mean:

> "SQL Server saved a copy of my entire database."

It means the operations within that transaction are treated as an atomic unit.