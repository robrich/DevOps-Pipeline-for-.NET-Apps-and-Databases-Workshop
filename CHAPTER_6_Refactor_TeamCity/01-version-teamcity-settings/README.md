SAVE TEAMCITY CONFIGURATION TO SOURCE CONTROL
=============================================

TeamCity can save all it's changes to source control so you can keep track of how build configurations evolve.

1. Create `C:\git-server\TeamCity` folder.

2. Open a terminal in this folder.

3. Type `git init --bare`.

4. Close the terminal.

5. In TeamCity dashboard, click `Administration`.

6. Click `Projects`.

7. Click `Root Project`.

8. Click `VCS Roots`.

9. Click `Create VCS Root`.

10. Name the root `TeamCity`.

11. Set the `Fetch URL` to `C:\git-server\TeamCity`.

   ![TeamCity VCS Root](1-teamcity-vcs-root.png)

12. From the menu on the left, choose `Versioned Settings` towards the bottom.

13. Click `Synchronization enabled`.

14. Choose the `TeamCity` VCS root.

15. Click on `Show settings changes in builds`.

16. Click Apply.

    ![Versioned Settings](2-versioned-settings.png)
