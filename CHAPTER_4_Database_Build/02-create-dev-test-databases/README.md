CREATE DEV AND TEST DATABASES
=============================

SQL Change Automation is great at evolving database schemas, but we want more control over database creation.  Specifically, we want each database environment to use unique credentials.  This best practice ensures the app doesn't accidentally connect to the wrong environment -- the password won't work.


Create Dev Database and User
----------------------------

1. Start -> SQL Server Management Studio (SSMS)

2. Connect to the local server: `.`.

3. Right-click on Databases, and choose `New Database`.

4. Enter the database Name `TodoStoreDev`, and click Create.

5. Inside the Security group at the bottom, right-click on Logins and choose `New Login`.

6. In the General page:

   a. Enter the Login Name as `SqlChangeAutomationDev`.
   
   b. Change type to `SQL Server Authentication`.
   
   c. Enter a password.
   
   d. Uncheck `Enforce password policy`.
   
   e. Change the Default database to `TodoStoreDev`.

7. Switch to the `User Mapping` page, and set:

   a. Check the `TodoStoreDev` database created above.

   b. Set default schema to `dbo`.

   c. Check `db_owner` because SQL Change Automation will create tables.

8. Click `OK` to create the user.


Create Test Database and User
-----------------------------

1. Right-click on Databases, and choose `New Database`.

2. Enter the database Name `TodoStoreTest`, and click Create.

3. Inside the Security group at the bottom, right-click on Logins and choose `New Login`.

4. In the General page:

   a. Enter the Login Name as `SqlChangeAutomationTest`.
   
   b. Change type to `SQL Server Authentication`.
   
   c. Enter a password.
   
   d. Uncheck `Enforce password policy`.
   
   e. Change the Default database to `TodoStoreTest`.

5. Switch to the `User Mapping` page, and set:

   a. Check the `TodoStoreTest` database created above.

   b. Set default schema to `dbo`.

   c. Check `db_owner` because SQL Change Automation will create tables.

6. Click `OK` to create the user.


Test it out
-----------

1. In SSMS, at the top of the Object Explorer window, click `Connect`.

2. The Server name is `.`.

3. Change Authentication to `SQL Server Authentication`.

4. Enter the Dev username and password from above.

5. Click Connect.

If it authenticates correctly, it works.  Now repeat the process for the Test user and database.
