```sql
CREATE TABLE Registrations
(
    Id INT IDENTITY(1,1) NOT NULL
        CONSTRAINT PK_Registrations PRIMARY KEY,

    PhoneNumber NVARCHAR(20) NOT NULL,

    FirstName NVARCHAR(100) NOT NULL,

    LastName NVARCHAR(100) NOT NULL,

    Gender INT NOT NULL,

    Grade INT NOT NULL,

    CreatedAt DATETIME2 NOT NULL
);
```

```sql
CREATE UNIQUE INDEX IX_Registrations_PhoneNumber
ON Registrations(PhoneNumber);
```
