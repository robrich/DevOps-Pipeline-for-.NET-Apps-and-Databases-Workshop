CREATE BUILD SCRIPT
===================

Though we could use build server plugins, this is fragile:

- Updating or rebuilding the build server requires reinstalling plugins.

- All projects must use the same version of each plugin.

- All plugins from all projects must be compatible.

To avoid these pitfalls, we'll use the build server solely to check the git repository and run a script.  Everything else will be done in this script and versioned together with our app.  Want to build an old version of the software?  Just check out that version and run the build script.


Elements of the Script
----------------------

Like the previous TeamCity build, we'll need the script to do these things:

1. Restore NuGet packages

2. Build the solution

3. Run the unit tests

4. Publish the website

5. Create the Octopus package

6. Push the package to the Octopus server

7. Deploy the package to the Dev site

Hint: how did we identify these tasks?  It's the build steps we created previously.


Writing the build script
------------------------

We could choose Powershell or Make or Gulp or Cake or CSscript, but for simplicity, let's use a batch file.

1. Create a new file named `build.bat` right next to `Site.sln` in the new folder.

2. Open `build.bat` in your favorite text editor.

3. Add this line to the top:

   ```bash
   pushd %~dp0
   ```

   This line says "cd to the directory the script is in".  This shouldn't be necessary, but it's a good defensive coding strategy.

4. Add this line:

   ```bash
   if "%BUILD_CONFIGURATION%"=="" set BUILD_CONFIGURATION=Release
   ```

   We'll use the environment variable `BUILD_CONFIGURATION` in each necessary step.  This command sets a default value if it wasn't set.  Though our build will always set it, developers running the script from the command line may not.  This is another good defensive coding strategy.

2. Add the NuGet restore line to the end of the file:

   ```bash
   dotnet restore || exit /b 1
   ```

   the `exit /b 1` piece says "if the command returns non-0, stop the script with exit code 1".  This will be TeamCity's signal to fail the build.

   Without this, the task would silently fail and the build would continue to the next line.

3. Add the Build line:

   ```bash
   dotnet build --configuration %BUILD_CONFIGURATION% || exit /b 1
   ```

   We can grab this command from the last successful build log from Chapter 1.  That's a great way to discover the commands and parameters to use.

   We're again checking that the command succeeded, and only proceeding if it did.

4. Add the Run tests commands:

   ```bash
   dotnet test Site.Tests\Site.Tests.csproj --configuration %BUILD_CONFIGURATION%  --logger "trx;LogFileName=Site.Tests.trx" || exit /b 1
   ```

   The `--logger` parameter writes the test results to an xml file inside the `Site.Tests/TestResults` folder.  Later we'll teach TeamCity to process this file and show the test results in the dashboard.

5. Add this line to publish the website:

   ```bash
   dotnet publish Site\Site.csproj --configuration %BUILD_CONFIGURATION% --output dist || exit /b 1
   ```

   This publishes the site to the `Site/dist` folder.  Though we could accept the default location of `bin/Release/netcoreapp2.2/publish/`, that's hard to remember.

6. Add this line to create the Octopus Deploy package:

   ```bash
   octo.exe pack --id BuildSite --format nupkg --version %BUILD_NUMBER% --basePath Site\dist --outFolder dist || exit /b 1
   ```

   Notice that we're using the `BUILD_NUMBER` environment variable.  TeamCity makes this available to all scripts.

   Why call it `BuildSite`?  Because naming is hard.  :D

7. Add this line to publish the Octopus package to the server:

   ```bash
   octo.exe push --server http://localhost:8090/ --package dist/BuildSite.%BUILD_NUMBER%.nupkg --apikey %OCTO_KEY% || exit /b 1
   ```

   We're also using `OCTO_KEY` environment variable here -- the API key we use to authenticate to the Octopus server.

8. Add this line to create the release and deploy it to the Dev environment.

   ```bash
   octo.exe create-release --server http://localhost:8090/ --project BuildSite --version %BUILD_NUMBER% --deployto Dev --waitfordeployment --apikey %OCTO_KEY% || exit /b 1
   ```

   We pretty much copied this line from the old build log.

9. Optional: If you find you're missing an environment variable or don't remember what it's called or don't know the value, you can add this line towards the top of the build script:

   ```bash
   set
   ```

   This will dump all environment variables to the console output.

And with that we're done with the script.


Commit the Script
-----------------

1. Open a command prompt in the source code directory.

2. Type these commands to add the build script to source control:

   ```bash
   git add .gitignore
   git add build.bat
   git commit -m "add build script"
   git push origin master
   ```

Test it out
-----------

We can't test it yet because we don't have the Octopus project and deployment steps in place yet.  Sadly, this means we can't discover typos yet.
