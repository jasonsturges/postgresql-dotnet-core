# PostgreSQL Dotnet Core

Convert an ASP.NET Core project to use PostgreSQL with Entity Framework.

This enables developer of ASP.NET Core projects on Mac OS X / macOS or linux targets.


### Installing PostgreSQL on Mac

Use brew to install PostgreSQL:

    $ brew install postgresql
    
Launch PostgreSQL service:

    $ brew services start postgresql


### Update appsettings.json

Configure connection string in project's appsettings.json, replacing the `username` and `dbname` appropriately:

    "ConnectionStrings": {
        "DefaultConnection": "User ID=username;Password=;Server=localhost;Port=5432;Database=dbname;Integrated Security=true;Pooling=true;"
    },


### Install NuGet packages

Install the following NuGet package in the ASP.NET web application project:

    Npgsql.EntityFrameworkCore.PostgreSQL

To do this, edit the project's .csproj file and add the following line in the `PackageReference` item group:

    <PackageReference Include="Npgsql.EntityFrameworkCore.PostgreSQL" Version="1.1.0" />


### Modify Startup.cs

Inside Startup.cs `ConfigureServices()` method, replace the `UseSqlServer` option with PostgresSQL:

    public void ConfigureServices(IServiceCollection services)
    {
        // Add framework services.
        services.AddDbContext<ApplicationDbContext>(options =>
            options.UseNpgsql(Configuration.GetConnectionString("DefaultConnection")));
    

### Run Entity Framework Migrations

Execute the following comment inside the project directory, **where the csproj file is located**:

    $ dotnet ef database update

After running the migration, the database is created and web application is ready to be run.
