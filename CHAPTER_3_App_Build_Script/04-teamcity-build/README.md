TEAMCITY BUILD
==============

We've already got TeamCity running on [port 8080](http://localhost:8080).  Let's create a build.

Much of the initial setup is identical to our previous build, but instead of creating distinct steps, we'll create a single build step to run `build.bat`.


Configure
---------

1. Open the TeamCity portal at http://localhost:8080 and login if necessary.

2. Click `Administration` on the top right.

3. Click `Projects` on the left menu if you're not there already.

4. Click `Create project`.

5. Click `Manually`.  Though the "From a repository URL" is enticing, our repository url is just a local folder, so it's difficult for TeamCity to discover things about it.

6. Enter the project name `.NET Build Script` and the Project ID will get filled out automatically.

7. Click `Create`.

8. In the next screen, click `Create build configuration`.

9. Again click `Manually`, enter the name `.NET Build Script`, and the Build configuration will get filled out automatically.

10. Click `Create`.


Create link to Version Control
------------------------------

A "VCS Root" is a link to the source control repository the build will use.

1. From the `Create new VCS Root` menu choose `Git`.

2. Enter `.NET Build Script` as the `VCS Root name`.

3. Enter the Fetch URL as `C:\git-server\BuildSite`.

4. Note the `Branch specification` field.  We won't change that here, but we could set it to match certain branch names if we want a build to only target the `master` branch for example.

5. Click `Advanced Options`.

6. Change `Clean policy` to `Always`.

   This ensures previous built content -- dlls, Octopus packages, etc -- gets deleted between builds.

7. Optional: change `Username style` to `Author Name and Email`.

8. Click `Create`.


Create Build Steps
------------------

1. Choose `Build Steps` from the menu on the left.

2. Though the `Auto-detect build steps` option looks mighty tasty, it'll notice the Visual Studio Solution file and get confused.  Let's do it by hand instead.

3. Click `Add Build Step`.

4. Choose `Command Line`.

5. Optional: enter a descriptive step name such as `build script`

6. Change `Run` to `Executable with parameters`.

7. In the `Command executable` field enter `build.bat`.

8. Click `Save` at the bottom.


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


Report unit tests
-----------------

Because we're not using the TeamCity task, it doesn't know we've produced test results in our build.  Let's teach it how to parse the `Site.Tests.trx` file.

1. Click `Build Features` on the left.

2. Click `Add build feature`.

3. Choose `XML report processing` from the bottom of the list.

4. Change `Report type` to `MSTest`.

   Yes, technically our tests are xUnit tests, but the logger logs in MSTest format.

5. In `Monitoring rules`, we'll give it a loose path to test results:

   ```
   **/TestResults/*.trx
   ```

   `**` is a wildcard that means "any directory nested at any level".  This pattern will automatically discover other test projects as we create them -- no need to modify the build.

6. Click `Save` at the bottom.


Release mode parameter
----------------------

1. Click on `Parameters` on the left menu.

2. Click `Add new parameter`.

3. Set `Name` to `BUILD_CONFIGURATION`.

4. Set `Kind` to `Environment variable`.

   This will change the name to `env.BUILD_CONFIGURATION`.  This is ok.

5. Set `Value` to `Release`.

6. Click `Save`.


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

We haven't built the Octopus details yet, so this build will fail, but we can see how far it'll get.

1. Click `Run` at the top-right to kick off a new build.

2. Click `Projects` or the TeamCity logo on the top-left to switch to the dashboard, then click on the newly running build.

3. Click on the `Build Log` tab and you can watch in real-time as the build executes.

Did the build fail creating the Octopus release?  If not, look at the build log, and look for typos in the build script or configuration errors in the TeamCity setup.

When you're done, the build will still fail, but it'll fail at the build script line `octo create-release ...`.
