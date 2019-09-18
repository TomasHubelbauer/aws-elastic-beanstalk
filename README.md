# AWS Elastic Beanstalk

- Scaffold a new .NET Core MVC app with individual account auth:
  `dotnet new mvc --auth Individual`
- Run the app using `dotnet run` and accept the self-signed certificate in the
  browser at https://localhost:5001
- Register, sign out, sign in and sign out again to test the database
- Remove `app.db` and `ConnectionStrings` in `appSettings.json`
- Install the EF in-memory provider:
  `dotnet add package Microsoft.EntityFrameworkCore.InMemory`
- Replace `AddDbContext` with
  `services.AddDbContext<ApplicationDbContext>(options => options.UseInMemoryDatabase(nameof(aws_elastic_beanstalk)))`
- *Could not load type 'Microsoft.EntityFrameworkCore.Infrastructure.IDbContextOptionsExtensionWithDebugInfo' from assembly 'Microsoft.EntityFrameworkCore, Version=3.0.0.0*

## To-Do

- [ ] Set up a workflow for publishing to EB from GitHub Actions
