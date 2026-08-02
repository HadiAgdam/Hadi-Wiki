

```powershell
Install-Package Dapper
Install-Package Microsoft.Data.SqlClient
```


```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=TemporaryRegistrationDb;User Id=sa;Password=YourPassword;TrustServerCertificate=True;"
  }
}
```


Using Dapper we should Create table using SQL query:
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



### Usage

```c#
using Dapper;
using Microsoft.Data.SqlClient;

public class RegisterService : IRegisterService
{
    private readonly IConfiguration _configuration;

    public RegisterService(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public async Task<bool> RegisterAsync(
        RegisterFormViewModel viewModel)
    {
        var connectionString =
            _configuration.GetConnectionString("DefaultConnection");

        await using var connection =
            new SqlConnection(connectionString);

        const string sql = """
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
            );
            """;

        try
        {
            var affectedRows = await connection.ExecuteAsync(
                sql,
                new
                {
                    viewModel.PhoneNumber,
                    viewModel.FirstName,
                    viewModel.LastName,
                    viewModel.Gender,
                    viewModel.Grade,
                    CreatedAt = DateTime.UtcNow
                });

            return affectedRows > 0;
        }
        catch (SqlException ex) when (ex.Number == 2601 || ex.Number == 2627)
        {
            // Duplicate phone number
            return false;
        }
    }
}
```
