STATIC DATA
===========


The problem
-----------

1. Open up SSMS and login to the local instance.

2. Open up the `TodoStoreDev` database.

3. Open up `Tables` group.

4. Right click on the `dbo.TaskStatus` table, and choose `Edit top 100 rows`.

5. Uh-oh. The table is empty.

We migrated the schema into place, but we didn't migrate the data.


New Migration Script
--------------------

Sadly, static data isn't baked into SQL Change Automation (yet).  See https://documentation.red-gate.com/display/SCA3/Static+Data for options.

1. Open up the SQL Change Automation tab in SSMS from the Tools menu.

2. If not there already, open the `TodoStore` workspace.

3. If not there already, click `Generate Migrations` in the TodoStore project.

4. Click the `Migrations` tab at the top of the window.

5. Right-click on the `Migrations` folder in the list, and choose `Add Migration`.

6. Optional: change the name of the migration file to something descriptive.  Be careful to preserve the number and date/time at the beginning of the script.

7. Click the migration to open it in a query window.

8. After the `-- <Migration ID=...` line, add this content:

   ```sql
   -- <Migration ID="460e3e7e-0af9-4c27-92cf-39c82188415e" />
   IF EXISTS ( SELECT * FROM dbo.TaskStatus WHERE Id = 1 )
     UPDATE dbo.TaskStatus SET Description = 'Unstarted' WHERE Id = 1
   ELSE
     INSERT INTO dbo.TaskStatus ( Id, Description )
     VALUES ( 1, 'Unstarted' )
   GO
   
   IF EXISTS ( SELECT * FROM dbo.TaskStatus WHERE Id = 2 )
     UPDATE dbo.TaskStatus SET Description = 'InProcess' WHERE Id = 2
   ELSE
     INSERT INTO dbo.TaskStatus ( Id, Description )
     VALUES ( 2, 'InProcess' )
   GO
   
   IF EXISTS ( SELECT * FROM dbo.TaskStatus WHERE Id = 3 )
     UPDATE dbo.TaskStatus SET Description = 'Complete' WHERE Id = 3
   ELSE
     INSERT INTO dbo.TaskStatus ( Id, Description )
     VALUES ( 3, 'Complete' )
   GO
   
   IF EXISTS ( SELECT * FROM dbo.TaskStatus WHERE Id = 4 )
     UPDATE dbo.TaskStatus SET Description = 'Deleted' WHERE Id = 4
   ELSE
     INSERT INTO dbo.TaskStatus ( Id, Description )
     VALUES ( 4, 'Deleted' )
   GO

   -- TODO: consider this carefully: DELETE FROM dbo.TaskStatus WHERE Id NOT IN ( 1, 2, 3, 4 )
   ```

   Yes, at best this script is a redundant mess.  Consider the other options at https://documentation.red-gate.com/display/SCA3/Static+Data until this is a well paved path.

9. Save the file.

10. Back in the SQL Change Automation window, switch to the `Apply to database` tab.

    Note our new migration script ready to run.

11. Click `Apply to database` then `Done`.

12. Optional: switch to the `Verify` tab and click the `Verify` button.


Commit to source control
------------------------

1. Open a terminal in the `TodoStore` folder -- the folder with the SQL scripts in it.

2. Add the new migration script to Git, and push the changes to the Git server:

   ```bash
   git add .
   git commit -m "TaskStatus data"
   git push origin master
   ```


Build
-----

1. Open the TeamCity dashboard at http://localhost:8080/.  Is there a pending or running build for the `TodoStore Database` project?

2. When the build goes green, choose the `TodoStoreDev` database, and on the `dbo.TaskStatus` table, choose `Edit top 100 rows`.

   The database now has content!


Test
----

How do we get this data into the Test database?

Hint: open the Octopus dashboard at http://localhost:8090/
