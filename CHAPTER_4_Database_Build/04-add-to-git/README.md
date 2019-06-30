ADD DATABASE TO GIT
===================

The folder we created in Step 2: Create Migration Scripts now has the migration scripts in it.  Let's save this folder to Git, and push it to our "git server" (the folder).


Save content to Git
-------------------

1. Open a command prompt in the root `TodoStore` folder you created in Step 2.

2. Run this command to generate a file Git will use to not commit certain files:

   ```bash
   echo bin > .gitignore
   ```

5. Open the `.gitignore` file and add these lines:

   ```text
   bin 
   obj
   *.user
   *.dbmdl
   *.jfm
   .vs
   .vscode
   ```

   Optional: add other files to this list that you'd like to ignore such as `*.log`, `*.debug`, etc.

6. Create a new Git repository and commit the content:

   ```bash
   git init
   git add .
   git commit -m "Initial Commit"
   ```


Create the Git "server"
-----------------------

1. Create a folder named `TodoStore` inside `C:\git-server`.

2. Open a terminal in this folder and run `git init --bare`.

3. Close the terminal.


Configure
---------

1. Return to the command window in the `TodoStore` folder we created in Step 2.

2. Type `git remote add origin c:\git-server\TodoStore`.  This creates a link from our project to the "server".

3. Type `git push -u origin master`.  This pushes the source code to the server and links the current local branch to the server's branch.


Test it out
-----------

1. Type `git pull`.

2. If you get no errors and it says `Already up to date`, it works.
