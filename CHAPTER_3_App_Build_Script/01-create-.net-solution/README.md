CREATE ANOTHER .NET SOLUTION
============================

We could use Visual Studio or Visual Studio Code to scaffold out a new project, but let's use the `dotnet` CLI to do it quickly instead.


1. Create a new empty folder in a convenient location -- your desktop or `C:\Users\you\Source\Repos`

2. Open a command prompt in this folder.

3. Run these commands to scaffold an MVC project, a unit test project, and add them to a solution file:

   ```bash
   dotnet new mvc --name Site --output Site
   dotnet new xunit --name Site.Tests --output Site.Tests
   dotnet new sln --name Site
   dotnet sln Site.sln add Site/Site.csproj
   dotnet sln Site.sln add Site.Tests/Site.Tests.csproj
   ```

4. Run this command to generate a file Git will use to not commit certain files:

   ```bash
   echo bin > .gitignore
   ```

5. Open the `.gitignore` file and add these lines:

   ```text
   bin 
   obj
   *.user
   .vs
   .vscode
   TestResults
   *.trx
   dist
   ```

   Optional: add other files to this list that you'd like to ignore such as `*.log`, `*.debug`, etc.

6. Create a new Git repository and commit the content:

   ```bash
   git init
   git add .
   git commit -m "Initial Commit"
   ```

Test it Out
-----------

1. Open a command prompt in the directory with the solution file.

2. Restore nuget packages:

   ```bash
   dotnet restore
   echo %ERRORLEVEL%
   ```

   If it returns `0`, the nuget restore worked.

   `ERRORLEVEL` is the command prompt's way of showing the exit code from the program.  Most programs reply `0` on success and non-`0` on failure.  We'll use this in our build script.

3. Build the solution in release mode:

   ```bash
   dotnet build -c Release
   echo %ERRORLEVEL%
   ```

   If it returns `0`, the build succeeded.

4. Run the unit tests:

   ```bash
   cd Site.Tests
   dotnet test
   echo %ERRORLEVEL%
   ```
   
   If it returns `0`, the tests passed.

5. Run the unit tests and harvest results:

   ```bash
   dotnet test --logger "trx;LogFileName=Site.Tests.trx"
   echo %ERRORLEVEL%
   ```

   If it returns `0`, the tests passed.

   You can now also look at the test output by opening the `TestResults/Site.Tests.trx` file.  We'll teach TeamCity to gather this file.
