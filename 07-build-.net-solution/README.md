BUILD .NET SOLUTION
===================

We're going to flex the Microsoft Build tools to build a website and run unit tests.

Background
----------

This project is just a new ASP.NET MVC website using .NET 4.5.2 with OctoPack NuGet package, and a companion unit test project switched to use XUnit.  You can find this solution on GitHub at [https://github.com/robrich/CICD-Sample-App](https://github.com/robrich/CICD-Sample-App).


Contents of the Zip File
------------------------

1. File -> New Project

2. ASP.NET Web Application (not core)

3. MVC project template

4. Authentication to No Authentication

5. Add Unit Tests

6. Add these NuGet packages:

  - xunit - the test library
  - xunit.runner.visualstudio - for the Test Explorer window in VS
  - xunit.runner.console - to run tests from the command line
  - FluentAssertions - asserts read like sentences
  - OctoPack - used by Octopus Deploy to build assets

7. Change Home Controller Tests to use XUnit instead of MSTest

8. Commit this to a local git repository.


Setup
-----

1. Create a new folder in a conspicuous spot.

2. Unzip `.NET Website source code.zip` into this folder.


Run
---

1. Open a command prompt in the directory with the solution file.

2. Type `nuget restore`.

3. Type `echo %ERRORLEVEL%`.  If it returns `0` the nuget restore worked.

4. Type `msbuild /m /p:Configuration=Release`.  `/m` says "use all CPUs".

5. Type `echo %ERRORLEVEL%`.  If it returns `0` the build worked.

6. Inside `packages\xunit.runner.console.2.2.0\tools` you'll find `xunit.console.exe`, the test runner.  It's a NuGet package, so `nuget restore` brought it in.

7. Type `packages\xunit.runner.console.2.2.0\tools\xunit.console.exe WebApplication1.Tests\bin\Release\WebApplication1.Tests.dll -nunit WebApplication1.Tests.xml` to run the tests and output the results in NUnit format to an xml file.

8. Type `echo %ERRORLEVEL%`.  If it returns `0` all the tests passed.


Other Options
-------------

- If you're building .NET 4.6, you'll need [.NET Targeting Pack](https://www.microsoft.com/net/targeting) for the target .NET version.  If you're building .NET 3.5 or earlier, you'll need the [.NET SDK](http://getdotnet.azurewebsites.net/target-dotnet-platforms.html) for the target .NET version.
