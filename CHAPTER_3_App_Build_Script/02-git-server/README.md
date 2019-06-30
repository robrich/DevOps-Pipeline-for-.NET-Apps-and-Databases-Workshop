GIT SERVER
==========

Like we did in the previous project, we'll use a local folder as a "git server".

For production use, you'll look to [GitHub](https://www.github.com/), [BitBucket](https://www.bitbucket.com), Azure DevOps, or another cloud provider.  Or perhaps you'd like to run Git on-premise with servers like [WebGit.NET](https://github.com/otac0n/WebGitNet) or any of the servers listed [here](http://stackoverflow.com/questions/5507489/git-server-like-github)


Create
------

1. Create a folder named `BuildSite` inside `C:\git-server`.

2. Open a terminal in this folder and run `git init --bare`.

3. Close the terminal.


Configure
---------

1. Open a command window inside the source code project you created in Step 1: Create .NET Solution.

2. Type `git remote add origin c:\git-server\BuildSite`.  This creates a link from our project to the "server".

3. Type `git push -u origin master`.  This pushes the source code to the server and links the current local branch to the server's branch.


Test it out
-----------

1. Type `git pull`.

2. If you get no errors and it says `Already up to date`, it works.
