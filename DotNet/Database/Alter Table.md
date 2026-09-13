
```sql
ALTER TABLE Registrations
ADD Processed BIT NOT NULL
    CONSTRAINT DF_Registrations_Processed DEFAULT 0;

ALTER TABLE Registrations
ADD ProcessedAt DATETIME2 NULL;
```

```sql
CREATE INDEX IX_Registrations_Processed_Id
ON Registrations(Processed, Id);
```


```sql
ALTER TABLE TableName
ALTER COLUMN ColumnName NewDataType;
```