SQL Change Automation
=====================

SQL Change Automation allows us to track a database's schema over time, and automatically deploy these changes between environments.  As part of this workshop, we'll use the 28-day trial.

By default, SQL Change Automation (formerly Ready Roll) is available as a Visual Studio plugin.  We won't use it that way today.  Instead, we'll use the SSMS plugin -- currently in beta.


Main Install
------------

1. Download SQL Change Automation, part of the [SQL Toolbelt](https://www.red-gate.com/dynamic/products/sql-development/sql-toolbelt/download).

2. Launch `SQLToolbelt.exe`.

3. Uncheck all products.

4. Check these products:

   - `SQL Change Automation`
   - `SQL Change Automation Powershell`
   - `SSMS Integration`

5. Click `Continue` and `Next` a few times until the installer finishes.

   If SQL Server Management Studio is open, the installer will prompt you to close it before it can continue.


SSMS Plugin Install
-------------------

1. Download the [SQL Change Automation SSMS Plugin](https://www.red-gate.com/products/sql-development/sql-change-automation/entrypage/ssms-addin)

2. Launch `SQLChangeAutomationSSMS_`bunch-a-numbers`.exe`.

3. Click next a few times, and finish to complete the installation.

   If SQL Server Management Studio is open, the installer will prompt you to close it before it can continue.


TeamCity Plugin Install
-----------------------

1. Download the [TeamCity plugin](https://www.red-gate.com/dlmas/TeamCity-download)

2. Launch the TeamCity dashboard at http://localhost:8080/

3. Login if necessary.

4. Click on Administration on the top-right.

5. On the bottom-left, choose `Plugins List`.

6. Click `Upload zip` and upload `sqlchangeautomation-teamcity.zip`.

7. Click `Restart server` to apply the changes.


MSBuild Tools
-------------

[SQL Change Automation](https://documentation.red-gate.com/sca3/automating-database-changes/automated-deployment-with-sql-change-automation-core/using-octopus-deploy-with-sql-change-automation-core) requires [MSBuild Tools](https://docs.microsoft.com/en-us/visualstudio/install/workload-component-id-vs-build-tools) 12 or greater.  Though MSBuild 4 is installed with Windows, we'll need the newer version.

1. Visit the [Visual Studio Downloads](https://visualstudio.microsoft.com/downloads/).

2. Scroll down to `All Downloads`, and open the `Tools for Visual Studio 2019` folder.

3. Download [Build Tools for Visual Studio 2019](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16)

4. Launch `vs_buildtools__`bunch-a-numbers`.exe`.  Sadly, this is only the launcher.  It will download around 800 megs of data by the time it's done.

5. After it downloads the installer files, the typical VS installer is shown.

6. The `Build Tools` option is already selected.  Click `Install` on the bottom-right.

7. Close the installer.

8. Start -> Services -> TeamCity.  Restart both TeamCity services to get the new `PATH` in place.


Test it out
-----------

1. Launch SQL Server Management Studio.

2. If it didn't open already, open the SQL Change Automation window from the Tools menu.

3. You should see the "Open or Create Workspace" window -- it works!
