

# 1. Why is Dapper faster than EF Core?

Let's imagine you execute:

```
SELECT * FROM Registrations WHERE PhoneNumber = @PhoneNumber
```

With **Dapper**, the flow is roughly:

```
Your C# code
    ↓
Dapper
    ↓
ADO.NET / SqlConnection
    ↓
SQL Server
    ↓
Results
    ↓
Map columns → C# object
```

Dapper is a **micro-ORM**. It does relatively little.

With EF Core, there is generally more infrastructure involved:

```
Your C# code
    ↓
EF Core
    ↓
LINQ expression
    ↓
Query translation
    ↓
SQL generation
    ↓
Change tracking
    ↓
ADO.NET
    ↓
SQL Server
    ↓
Materialization
    ↓
Change tracking
    ↓
C# object
```

The biggest reasons Dapper can be faster are:

### 1. Less abstraction

Dapper sits very close to ADO.NET.

You essentially write SQL yourself:

```
var user = await connection.QuerySingleOrDefaultAsync<User>(
    "SELECT * FROM Users WHERE Id = @Id",
    new { Id = id });
```

EF Core instead lets you write:

```
var user = await db.Users
    .FirstOrDefaultAsync(x => x.Id == id);
```

EF Core has to translate the LINQ expression and manage more ORM functionality.

---

### 2. No default change tracking

EF Core tracks entities by default.

For example:

```
var user = await db.Users
    .FirstAsync(x => x.Id == id);
```

EF Core may track that object so it can later detect:

```
user.FirstName = "John";

await db.SaveChangesAsync();
```

Dapper doesn't do this.

If you query:

```
var user = await connection.QuerySingleAsync<User>(
    "SELECT * FROM Users WHERE Id = @Id",
    new { Id = id });
```

Dapper simply maps the result to an object.

No tracking.

For a registration website where you mostly:

```
INSERT
SELECT EXISTS
```

you may not need EF Core's change tracking at all.

---

### 3. You control the SQL

With Dapper, you write the exact SQL that gets executed.

For example:

```
INSERT INTO Registrations
(
    PhoneNumber,
    FirstName,
    LastName,
    Gender,
    Grade,
    CreatedAt
)
VALUES
(
    @PhoneNumber,
    @FirstName,
    @LastName,
    @Gender,
    @Grade,
    @CreatedAt
)
```

This can be very efficient.

---

### 4. Lower memory overhead

Dapper generally has less overhead than a full ORM because it doesn't need to maintain the same level of entity state and tracking.

This can matter when you have many concurrent requests.

