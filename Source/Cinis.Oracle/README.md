# Cinis.Oracle

Dapper(Oracle) extensions for .Net projects

## Installation

```
NuGet\Install-Package Cinis.Oracle -Version 1.0.0
```

## Usage

An entity must be defined with specified DataAnnotations

```cs
[Table("user", Schema = "mydb")]
public class User
{
    [Key]
    [Column("id")]
    public int Id { get; set; }

    [Column("username")]
    public string Username { get; set; }

    [Column("password")]
    public string Password { get; set; }

    [Column("date_of_birth")]
    public DateTime DateOfBirth { get; set; }
}
```

Create Operations

```cs
using (var conn = new OracleConnection("{connection string}"))
{
    conn.Open();
    using (OracleTransaction tran = conn.BeginTransaction())
    {
        try
        {
            User user = new()
            {
                Username = "username",
                Password = "Password",
                DateOfBirth = DateTime.Now
            };
            var result = conn.Create(user);
            tran.Commit();
        }
        catch (Exception ex)
        {
            // Roll the transaction back
            tran.Rollback();
            // Handle the error however you need to.
            throw;
        }
    }
}
```