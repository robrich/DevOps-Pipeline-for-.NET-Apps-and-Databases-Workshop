APP BUILD SCRIPT
================

In the previous chapter, we built the DevOps pipeline as steps in TeamCity.  This yields the highest fidelity build experience, but means we're tied to TeamCity.  What if our company suddenly moves to Jenkins or Azure DevOps?

In this chapter, we'll move all the build tasks to a build script, and create a new TeamCity build that'll merely run the script.

If you want to "debug" the build, you can just run the build script from a command prompt.
