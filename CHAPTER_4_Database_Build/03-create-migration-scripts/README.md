CREATE MIGRATION SCRIPTS
========================

In this section we'll save the database schema as migration scripts using RedGate SQL Change Automation.  In Chapter 1, we installed SQL Change Automation, and started the free 30-day trial.

We could choose to version tables, views, stored procedures, functions, etc. with SQL Change Automation.  In this example we only have tables, so that's all we versioned.


Create the SQL Change Automation project
----------------------------------------

1. Create an empty folder in a convenient location -- your desktop or `C:\Users\you\Source\Repos`.  Ensure this folder isn't inside the other folders we've built today.

2. Name the folder `TodoStore`.

3. If it isn't open already, start SQL Server Management Studio (SSMS) and login to the local database.

4. Open the SQL Change Automation tab from the Tools window.

5. Click `New Workspace`.

6. Name the workspace `TodoStore`.

7. Click `Browse` and locate the folder you created in step 1 above.

8. At the bottom, click `New database project`.

   In SQL Change Automation, a "workspace" is a group of databases.  In this case, we only have a single database, so the group is somewhat redundant.

9. Click `Select development database...`.

10. Choose the `TodoStore` database on your local machine, and push `OK`.

11. Set the `Database project name` to `TodoStore`.

12. Push `Next`.

13. We don't need any filter rules, so push `Skip` on the bottom-right.

14. "Baseline" is great when we're adding SQL Change Automation to an existing database.  We can tell the system "start from here".  In our case, we don't have an existing deployed database, so starting from blank is ok.

    In an established production system, skipping this step may delete the existing database on first build or the build would fail, being unable to create duplicate tables.

    Click `Skip and Finish` on the bottom-right.

15. On the left we see the database, on the right we see the "project" -- the SQL migration files to get it this way.  Right now the database has two tables, and the project is empty.

    Click `Generate Migrations` to discover these two tables.

16. Note the system discovered both tables.  Click `Generate migrations`.

17. Click `Done` to finish the schema sync.

18. In Windows Explorer, look in the `TodoStore` we created at the beginning of this step.  It now has the migration scripts.

19. Because we skipped the baseline, we'll need to adjust the project to confirm this.  Open `TodoStore/TodoStore/TodoStore.sqlproj` in your favorite text editor.

20. At the bottom of the first `<PropertyGroup>` (just above the first `</PropertyGroup>`) add this line:

    ```xml
    <SkipBaselineCheck>True</SkipBaselineCheck>
    ```

21. Save and close the file.


Shadow Database
---------------

If you refresh the list of databases in SSMS, you'll now see a `TodoStore_you_SHADOW` database.  This database is used internally by SQL Change Automation.  You can delete this if you'd like, but it'll return next time you use SQL Change Automation.  I find it best to ignore it.
