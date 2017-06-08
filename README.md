# ASPNetMVCMySQLIdentity
Use ASP.Net identity provided with MySQL database

I have tested it using Visual Studio 2013 (Update 5) and MySQL version 5.5.10


Visual Studio

a. MVC Application
- Create a new project  File --> New Project
- From the list of templates, select 'Web' and then 'ASP.Net Web Application'.
- Next select 'MVC' template with the default option, the authentication method used will be 'Individual User Accounts'. Select 'OK'

b. Install 'MySQL.Data.Entity'
- Using Nuget Package Manager install 'MySQL.Data.Entity', which is the MySQL provider for EntityFramework

c. 'Web.Config' file
- Open Web.Config file
- In the section '<connectionStrings>', rename the existing connection called 'DefaultConnection' to something else.
- Add a new connection called 'DefaultConnection' with providerName="MySql.Data.MySqlClient". This connection is the one that will be used to connect to MySQL database. Check the database name is 'dummymvc4' (same as the one where identity tables have been saved)
- change '<entityFramework>' to '<entityFramework codeConfigurationType="MySql.Data.Entity.MySqlEFConfiguration, MySql.Data.Entity.EF6">'

Now rebuild the solution


-- Attempt at generating tables from code behind
Open Package Manager Console from Tools --> Nuget Package Manager --> Package Manager Console
1. In the console, Type 'Enable-Migrations'
2. In the folder 'Migrations' open 'Configuration.cs' Under Configuration() add
SetSqlGenerator("MySql.Data.MySqlClient", new MySql.Data.Entity.MySqlMigrationSqlGenerator());

3. Open the file 'IdentityModels.cs' in the folder 'Models'. In ApplicationDbContext add
        
        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            base.OnModelCreating(modelBuilder);

            modelBuilder.Entity<ApplicationUser>().Property(u => u.UserName).HasMaxLength(128);

            //Uncomment this to have Email length 128 too (not neccessary)
            //modelBuilder.Entity<ApplicationUser>().Property(u => u.Email).HasMaxLength(128);

            modelBuilder.Entity<IdentityRole>().Property(r => r.Name).HasMaxLength(128);
        }
        
4. Rebuild the solution
5. Open Package Manager Console and type 'Add-Migration Initial' and then 'Update-Databse -Verbose'

All the identity tables will now be created in the database.




