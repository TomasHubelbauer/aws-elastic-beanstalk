# AWS Elastic Beanstalk

- Scaffold a new .NET Core MVC app with individual account auth:
  `dotnet new mvc --auth Individual`
- Run the app using `dotnet run` and accept the self-signed certificate in the
  browser at https://localhost:5001
- Register, sign out, sign in and sign out again to test the database
- Remove `app.db` and `ConnectionStrings` in `appSettings.json`
- Remove the `app.db` entry in `aws-elastic-beanstalk.csproj`
- Install the EF in-memory provider:
  `dotnet add package Microsoft.EntityFrameworkCore.InMemory`
- Replace the version in `aws-elastic-beanstalk` with the same (or closest) one
  as is the EF Core one from NuGet manually and run `dotnet restore` because
  `dotnet add package` doesn't support installing pre-relase packages
- Replace `AddDbContext` with
  `services.AddDbContext<ApplicationDbContext>(options => options.UseInMemoryDatabase(nameof(aws_elastic_beanstalk)))`
- Retry the flow end to end again
- Publish using `dotnet publish -r win-x64 --self-contained` because Elastic
  Beanstalk probably has a different runtime version so we need to produce a
  self-hosted package to avoid the *ANCM In-Process Handler Load Failure* error
- Run `Compress-Archive bin\Debug\netcoreapp3.0\win-x64\publish awsebs.zip` to
  produce an archive of a source code version for EBS
- Go to https://us-east-2.console.aws.amazon.com/elasticbeanstalk/home and click
  *Get started*
- Select .NET as platform, select *Upload your code* and select the archive
- Wait for the app to spin up and go to *Configure* and switch to free t2.micro
  to remain on the free tier
- Retry end to end on http://awsebs-env.izbp7iptq3.us-east-2.elasticbeanstalk.com

## To-Do

### Add instructions for uploading using the AWS CLI

- `pip install awscli` https://aws.amazon.com/cli/
- `aws configure` https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html
  - Same region as EBS
- https://stackoverflow.com/a/41805285/2715716

### Set up a workflow for publishing to EB from GitHub Actions using the AWS CLI

### Add info on using SQLite on the EC2 instance (file based)

### Add info on connecting with RDS (managed)

### Add info on carrying out migrations in EF Core
