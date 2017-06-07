# ASPNetMVCMySQLIdentity
Use ASP.Net identity provided with MySQL database

I have tested it using Visual Studio 2013 (Update 5) and MySQL version 5.5.10

MySQL -
- create database schema 'dummymvc'
- Create identity tables using the SQL statments in the file 20170606_createtables_dummymvc.sql . All of the tables will be created in the database 'dummymvc'.

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
- Add a new connection called 'DefaultConnection' with providerName="MySql.Data.MySqlClient". This connection is the one that will be used to connect to MySQL database. Check the database name is 'dummymvc' (same as the one where identity tables have been saved)

Now rebuild the solution


-- Attempt at genrating tables from code behind

1. Migrations/Configuration.cs file 
Under Configuration() add
SetSqlGenerator("MySql.Data.MySqlClient", new MySql.Data.Entity.MySqlMigrationSqlGenerator());

Otherwise you will get the error -
No MigrationSqlGenerator found for provider 'MySql.Data.MySqlClient'. Use the SetSqlGenerator method in the target migrations configuration class to register additional SQL generators.

2. Web.config file
change '<entityFramework>' to '<entityFramework codeConfigurationType="MySql.Data.Entity.MySqlEFConfiguration, MySql.Data.Entity.EF6">'

Else you will get the error -
Specified key was too long; max key length is 767 bytes
so instead of 
create table `__MigrationHistory` (`MigrationId` nvarchar(150)  not null ,`ContextKey` nvarchar(300)  not null ,`Model` longblob not null ,`ProductVersion` nvarchar(32)  not null ,primary key ( `MigrationId`,`ContextKey`) ) engine=InnoDb auto_increment=0

the create table command will be 
create table `__MigrationHistory` (`MigrationId` nvarchar(150)  not null ,`ContextKey` nvarchar(300)  not null ,`Model` longblob not null ,`ProductVersion` nvarchar(32)  not null ,primary key ( `MigrationId`) ) engine=InnoDb auto_increment=0


-- for code first
http://www.thinkprogramming.co.uk/code-first-with-mysql-and-entity-framework-6/

add Migration History --
Open Tools --> Nuget Package Manager --> Package Manager Console
- Enable-Migrations 
- Add-Migration Initial
- Update-Database


if model has been changed then run the following commands from package manager console -
- Add-Migration Length_Constraints
- Update-Database


