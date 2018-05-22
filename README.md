# PostgreSQL ASP.NET Core 2.0

Convert an ASP.NET Core Web Application project to use PostgreSQL with Entity Framework.

This enables development of ASP.NET Core projects using [VS Code](https://code.visualstudio.com/) on Mac OS X / macOS or linux targets.

![vscode](http://labs.jasonsturges.com/coreclr/postgresql-dotnet-core.png)

This repository uses ASP.NET Core 2.0 Visual Studio 2017 ASP.NET Core Web Application project scaffold updated to use PostgreSQL.


## Project Setup

Project setup has already been completed in this repository.  

Below, instructions are referenced to use PostgreSQL in a ASP.NET Core project.


### Install NuGet packages

Install the `Npgsql.EntityFrameworkCore.PostgreSQL` NuGet package in the ASP.NET web application.

To do this, you can use the `dotnet` command line by executing:

    $ dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL --version 2.0.2

Or, edit the project's .csproj file and add the following line in the `PackageReference` item group:

    <PackageReference Include="Npgsql.EntityFrameworkCore.PostgreSQL" Version="2.0.2" />


### Update appsettings.json

Configure connection string in project's appsettings.json, replacing the `username`, `password`, and `dbname` appropriately:

    "ConnectionStrings": {
        "DefaultConnection": "User ID=username;Password=password;Server=localhost;Port=5432;Database=dbname;Integrated Security=true;Pooling=true;"
    },


### Modify Startup.cs

Inside Startup.cs `ConfigureServices()` method, replace the `UseSqlServer` option with PostgresSQL:

    public void ConfigureServices(IServiceCollection services)
    {
        // Add framework services.
        services.AddDbContext<ApplicationDbContext>(options =>
            options.UseNpgsql(Configuration.GetConnectionString("DefaultConnection")));
    

## Running the solution

Before the solution can be executed, be sure to run entity framework migrations.


### Run Entity Framework Migrations

Execute the following comment inside the project directory, **where the .csproj file is located**:

    $ dotnet ef database update

After running the migration, the database is created and web application is ready to be run.


## Setting up a PostgresSQL server on Mac

Here are instructions to setup a PostgreSQL server on Mac using Homebrew.


### Installing PostgreSQL on Mac

Use [brew](https://brew.sh/) to install PostgreSQL, then launch the service:

    $ brew install postgresql
    $ brew services start postgresql


### Create a user

Create a user using the `createuser` command from a terminal.  Using the `-P` argument, you will be prompted to setup a password.

    $ createuser username -P


### Create a database

Create your database using the `createdb` command from a terminal.

    $ createdb dbname
    
At this time, run the solution's Entity Framework migrations (see above for instructions).


### Verifying database

Launch PostgreSQL interactive terminal and connect to the database.

    $ psql dbname


From the PostgreSQL interface terminal, List tables using the `\dt` command:

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
