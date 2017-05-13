SQL SERVER MANAGEMENT STUDIO (SSMS)
===================================

In past versions, SQL Server Management Studio (SSMS) was part of the SQL install disk.  It's now a separate download because it is now free where the SQL Server engine is still paid.

Install
-------

1. Download [SSMS](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)

2. Launch `SSMS-Setup-ENU.exe`.

3. Push next, and complete the install.  Because it uses the Visual Studio 2015 shell, this takes a long time.

4. Restart the machine when prompted.


Test it out
-----------

1. Start -> `Microsoft SQL Server Management Studio`

2. Connect to the default instance (use the server name or change to `.`)

3. If you get no errors, it works.

4. Let's return to SQL Server, and verify `TCP` connections work too.
