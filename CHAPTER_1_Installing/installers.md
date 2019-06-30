Installers
==========

There's a lot to install.  If you can, grab all these installers from the thumb drives going around.  If you're not in the room, you can download each installer from the URLs here.

Defaults
--------
Whenever possible, grab the 64-bit Windows version of each installer.


Git
---

http://git-scm.com/download

Get the 64-bit Windows edition

SQL Server
----------

https://www.microsoft.com/en-us/sql-server/sql-server-downloads

SSMS (SQL Server Management Studio)
-----------------------------------

https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms

This is no longer bundled with SQL Server installs

TeamCity
--------

https://www.jetbrains.com/teamcity/download/

Octopus Deploy 30-day trial
---------------------------

https://octopus.com/downloads

You'll want to grab each of these:
- Octopus Server (LTS, 64-bit)
- Octopus Tentacle (64-bit)
- Command Line (Windows 32-bit)

.NET Core 2.2 SDK
-----------------

https://dotnet.microsoft.com/download/dotnet-core

Grab the 64-bit Windows version

.NET Core on IIS
----------------

https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/index?view=aspnetcore-2.2#install-the-net-core-hosting-bundle is the docs and leads you to the **dotnet hosting bundle** download.

SQL Change Automation: 14-day trial
-----------------------------------

- SQL Toolbelt has the main product: https://www.red-gate.com/dynamic/products/sql-development/sql-toolbelt/download

- SSMS Plugin: https://www.red-gate.com/products/sql-development/sql-change-automation/entrypage/ssms-addin

- TeamCity plugin: https://www.red-gate.com/dlmas/TeamCity-download

- We'll install the Octopus Deploy SQL Change Automation plugin from the Octopus dashboard later.

Visual Studio Build Tools
-------------------------

SQL Change Automation requires MSBuild v12 or better.

1. Visit the [Visual Studio Downloads](https://visualstudio.microsoft.com/downloads/).

2. Scroll down to `All Downloads`, and open the `Tools for Visual Studio 2019` folder.

3. Download [Build Tools for Visual Studio 2019](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16)


Optional
--------

To help manage the server, you may choose to install other tools.  Here's some of my favorites:

- [7 Zip](http://www.7-zip.org/) - easier than the extract wizard in Windows. Launch `7z1900-x64.exe` to install.

- [Visual Studio Code](https://code.visualstudio.com/) - you could also choose Notepad++, Atom, Brackets, Visual Studio Code, or a number of others.  You'll want one that can preserve Unix-style line endings, so avoid Notepad. Launch `VSCodeUserSetup-`bunch-of-numbers`.exe` to install.

- [Chrome](https://www.google.com/chrome) - you could also choose Firefox, Edge, or any modern browser. Launch `ChromeSetup.exe` to install.
