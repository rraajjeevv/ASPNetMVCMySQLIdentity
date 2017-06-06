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



