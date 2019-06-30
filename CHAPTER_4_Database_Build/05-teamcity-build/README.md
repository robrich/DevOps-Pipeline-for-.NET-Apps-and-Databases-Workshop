TEAMCITY BUILD
==============

With the SQL Change Automation plugin installed in TeamCity and the database versioned and committed to Git, we're ready to build a DevOps pipeline.


Configure
---------

1. Open the TeamCity portal at http://localhost:8080 and login if necessary.

2. Click `Administration` on the top right.

3. Click `Projects` on the left menu if you're not there already.

4. Click `Create project`.

5. Click `Manually`.  Though the "From a repository URL" is enticing, our repository url is just a local folder, so it's difficult for TeamCity to discover things about it.

6. Enter the project name `TodoStore Database` and the Project ID will get filled out automatically.

7. Click `Create`.

8. In the next screen, click `Create build configuration`.

9. Again click `Manually`, enter the name `TodoStore Database`, and the Build configuration will get filled out automatically.

10. Click `Create`.


Create link to Version Control
------------------------------

A "VCS Root" is a link to the source control repository the build will use.

1. From the `Create new VCS Root` menu choose `Git`.

2. Enter `TodoStore Database` as the `VCS Root name`.

3. Enter the Fetch URL as `C:\git-server\TodoStore`.

4. Note that authentication to the Git repository is currently set to Anonymous. No change is needed here. If your source control repository was on GitHub or another provider, this would allow you to authenticate to their site.

5. Click `Advanced Options`.

6. Change `Clean policy` to `Always`.

   This ensures previous build results are deleted.

7. Optional: change `Username style` to `Author Name and Email`.

8. Click `Create`.


Create Build Steps
------------------

1. Choose `Build Steps` from the menu on the left.

2. Click `Add build step`.

3. Choose `Redgate SQL Change Automation Build`.

   Wait, what does "database build" mean?  It's SQL files.  We're not creating a `.dll`.

   SQL Change Automation uses the term "build" to mean "verify".  It creates a new blank database, runs all the migration scripts, and verifies it all works.  It "builds" the database from the scripts.

   This database is created fresh at the beginning of the build, and deleted at the end.  We'll never use this database.

4. Optional: enter a descriptive step name such as `build database`.

5. In the `Database project` step, enter the path `TodoStore\TodoStore\TodoStore.sqlproj`.  Yeah, it's nested a few folders in.

6. Set the `NuGet Package ID` to `TodoStore`.

   This is the package we'll send to Octopus Deploy.

7. In the `Temporary database` section, `SQL LocalDB` is the perfect choice.  We'll never see these temporary databases in SSMS since we connect to the other one.

8. Click on `Show advanced options`.

9. Note the `Override package version`.  Let's leave it blank and use the build number.  We'll need to set the build number in General Settings like we've done previously.

10. Click `Save` at the bottom.


Source change -> Build trigger
------------------------------

1. Choose `Triggers` from the menu on the left.

2. `Add a new trigger`.

3. Choose `VCS trigger`.

4. Optional: Imagine I've finished a task, and I commit then push a lot of times right in a row. Should TeamCity build each of these commits?  Or should it wait and only build the last commit?  There's two settings that can help here:

   a. `Trigger a build on each check-in`: leave this unchecked for now. It'll only build the latest commit it finds.

   b. `Quiet period`: leave this set to `Do not use` for now. You could choose to wait a minute or two. This could allow me to finish pushing all these changes, and only build the last one.

5. Click Save.

This will start the build every time TeamCity discovers a source code change.


Change Build Number
-------------------

Octopus wants a package number like `1.2.3`, and currently the build number is just `1` or `2`. Let's change the build number format.

1. Click on `General Settings` on the left.

2. If necessary, click `Show advanced options`.

3. Change `Build number format` to `1.0.%build.counter%`.

   Now for build #2, the build number will be `1.0.2`, for build 3 the build number will be `1.0.3`.  This isn't quite [Semantic Versioning](https://semver.org/) but it's close.

3. Click `Save` at the bottom.


Test it out
-----------

Now that we've got our build configured, let's run it.

1. Click on the TeamCity logo or `Projects` in the top-left.

2. Click `Run` on the far right of the build we created.

3. Click on the `Projects` link on the top-left or the TeamCity logo.

   In a minute or two the build will launch.

4. Did the build fail?  Take a look at the build step configuration.  Is there a typo in the path?

5. When the build goes green, celebrate! Congratulations!

6. Click on the `Success` link, and switch to the `Build Log` tab.

7. Open up each `>` section and take a look at the commands that are run.

   Hint: we may need these later.
