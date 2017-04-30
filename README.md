# PostgreSQL Dotnet Core

Convert an ASP.NET Core project to use PostgreSQL with Entity Framework.

This enables development of ASP.NET Core projects using [VS Code](https://code.visualstudio.com/) on Mac OS X / macOS or linux targets.

![vscode](http://labs.jasonsturges.com/coreclr/postgresql-dotnet-core.png)

This repository uses a Visual Studio 2017 ASP.NET project scaffold updated to use PostgreSQL.


### Installing PostgreSQL on Mac

Use [brew](https://brew.sh/) to install PostgreSQL, then launch the service:

    $ brew install postgresql
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


### Verifying database

Launch PostgreSQL interactive terminal:

    $ psql

From the PostgreSQL interface terminal, list databases using the `\l` command:

    username=# \l

                                             List of databases
         Name     |    Owner     | Encoding |   Collate   |    Ctype    |       Access privileges       
    --------------+--------------+----------+-------------+-------------+-------------------------------
     dbname       | jasonsturges | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
     

Connect to the newly created database:

    username=# \connect dbname


List tables using the `\dt` command:

    dbname=# \dt
                       List of relations
     Schema |         Name          | Type  |    Owner     
    --------+-----------------------+-------+--------------
     public | AspNetRoleClaims      | table | username
     public | AspNetRoles           | table | username
     public | AspNetUserClaims      | table | username
     public | AspNetUserLogins      | table | username
     public | AspNetUserRoles       | table | username
     public | AspNetUserTokens      | table | username
     public | AspNetUsers           | table | username
     public | __EFMigrationsHistory | table | username
    (8 rows)
   
