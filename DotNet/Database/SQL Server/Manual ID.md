

```sql
SET IDENTITY_INSERT ActivityTypes ON;

INSERT INTO ActivityTypes (Id, Name)
VALUES (1, 'CourseOpened');

SET IDENTITY_INSERT ActivityTypes OFF;
```

Turn on manual id, insert, turn it off again to make it do autoincrement again.