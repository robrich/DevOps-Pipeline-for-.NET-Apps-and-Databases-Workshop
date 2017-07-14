THE BUILD SCRIPT
================

Though we could use build server plugins, this is fragile:

- Updating or rebuilding the build server requires reinstalling plugins.

- All projects must use the same version of each plugin.

- All plugins from all projects must be compatible.

To avoid these pitfalls, we'll use the build server solely to check the git repository and run a script.  Everything else will be done in this script and versioned together with our app.  Want to build an old version of the software?  Just check out that version and run the build script.


Elements of the Script
----------------------

We need the script to do the following things:

1. Restore NuGet packages
1. Build the solution
1. Run the unit tests


Writing the build script
------------------------

We could choose Powershell or Make or Gulp or Cake or CSscript, but for simplicity, let's use a batch file:

1. Create `build.bat` using your favorite notepad replacement in the source code directory you crated in Step 7: BUILD .NET SOLUTION.
1. Add the NuGet restore lines:

	```bash
	nuget restore
	if not %errorlevel% equ 0 (
		exit /b %errorlevel%
	)
	```

	We use `exit /b %errorlevel%` to pass build errors to the calling program.  Without this, the build would silently fail and just continue to the next line.

1. Add the lines to build the solution:

	```bash
	msbuild WebApplication1.sln /m /p:Configuration=Release
	if not %errorlevel% equ 0 (
		exit /b %errorlevel%
	)
	```

	  `/m` says "use all cores"

1. Add these lines to run the unit tests:

	```bash
	packages\xunit.runner.console.2.2.0\tools\xunit.console.exe WebApplication1.Tests\bin\Release\WebApplication1.Tests.dll -nunit WebApplication1.Tests.xml
	if not %errorlevel% equ 0 (
		exit /b %errorlevel%
	)
	```

	When we run package restore, the xunit runner will get restored to `packages\xunit.runner.console.2.2.0\tools\xunit.console.exe`.

	`-nunit file.xml` will write the unit tests results in NUnit format to a file.  We'll tell the build server to pick up this file and show the unit test results.


Test it out
-----------

1. From a terminal in the source code directory, run `build.bat`.
1. Type `echo %errorlevel%`.  If it says `0`, it worked.


Add the file to source control
------------------------------

1. Open `.gitignore` in your favorite text editor.
1. Add a new line with `*.Tests.xml` so git will ignore test results.
1. Type these commands to add the build script to source control:

	```bash
	git add .gitignore
	git add build.bat
	git commit -m "add build script"
	```

1. Type `git push origin master` to send the changes to the git server.
