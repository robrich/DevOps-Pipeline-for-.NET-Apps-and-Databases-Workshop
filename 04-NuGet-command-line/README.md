NuGet Command Line
==================

NuGet command line allows us to run `nuget restore` as part of the build.


Install
-------

1. Download [NuGet Commandline](https://dist.nuget.org/index.html)

2. Create `NuGet` folder in `C:\Program Files (x86)`

3. Copy `nuget.exe` into the folder.


Configure
---------

Let's add NuGet to the `PATH` environment variable so commands can use it without specifying the full path.

1. Start -> Type `environment variables` to launch `Edit System Environment Variables`

2. Click `Environment Variables...`

3. In the lower `System` section, choose `PATH` and click `Edit`

![PATH environment var](1-path-env-var.png)

4. Add `C:\Program Files (x86)\NuGet` to the list.

5. Restart any services or terminals that depend on NuGet to update the `PATH` environment variable.


Test it out
-----------

1. Open a new command prompt in any directory.  *Note*: because we just changed the `PATH` environment variable, existing processes and shells won't be able to run msbuild.

2. Type `nuget`.

3. If you don't see any errors, it worked.
