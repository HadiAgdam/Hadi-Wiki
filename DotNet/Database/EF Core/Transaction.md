

```c#
private readonly DbContext _db;
```


```c#
await using var transaction = await _db.Database.BeginTransactionAsync(cancelationToken);

try
{
	// some code
	await _db.saveChangesAsync(cancelationToken);
	
	// ...
	
	// some more code
	await _db.saveChangesAsync(cancelationToken);
	
	// save all changes
	await transaction.CommitAsync(cancelationToken);
}
catch
{
	await transaction.RollbackAsync(cancelationToken);
}
```



```
BEGIN TRANSACTION
       ↓
   Operation 1
       ↓
   Operation 2
       ↓
   SaveChanges
       ↓
   Everything OK?
     ↙       ↘
   COMMIT   ROLLBACK
```



#### Important: `SaveChangesAsync()` already uses a transaction


When you call:

```
await _db.SaveChangesAsync();
```

EF Core normally wraps **that single SaveChanges operation** in a transaction automatically.

So you don't need to manually create a transaction every time you save.

You specifically want an explicit transaction when you have **multiple database operations that must succeed/fail together**, especially if there are multiple `SaveChangesAsync()` calls or other database operations between them.