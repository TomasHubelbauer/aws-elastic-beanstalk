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
- Archive the contents of `bin/Debug/netcoreapp3.0/win-x64/publish` together
  with the following `aws-windows-deployment-manifest.json` file and upload it
  to Elastic Beanstalk:
  ```json
  {
    "manifestVersion": 1,
    "deployments": {
      "aspNetCoreWeb": [
        {
          "name": "aws_elastic_beanstalk",
          "parameters": {
            "appBundle": ".",
            "iisPath": "/"
          }
        }
      ]
    }
  }
  ```
- Go to https://us-east-2.console.aws.amazon.com/elasticbeanstalk/home and click
  *Get started*
- Select .NET as platform, select *Upload your code* and select the archive
- Retry end to end on http://awsebs-env.izbp7iptq3.us-east-2.elasticbeanstalk.com

## To-Do

Add `aws-windows-deployment-manifest.json` to root and add
`<ItemGroup><None Source="aws-windows-deployment-manifest.json" CopyToOutput="Always"></ItemGroup>`
(like the snippet for `app.db` which was removed from `aws_elastic_beanstalk` prior)
so that each `dotnet deploy` automatically places the AWS manifest file in `publish`.

Add instructions for uploading using the AWS CLI.

Set up a workflow for publishing to EB from GitHub Actions using the AWS CLI.
