OCTOPUS DEPLOY BUILD
====================

Much of the Octopus configuration is already in place.  We don't need to setup environments, we only need to create the build, and add it to the server(s).

Can we move the Octopus release process into a script?  The Octopus Terraform provider is coming, but it's not here yet.  See https://octopus.com/docs/deployment-patterns/deployment-process-as-code  For now, we'll create the deployment process as tasks inside the Octopus UI.


Create a deployment project
---------------------------

1. Launch the Octopus Web Dashboard at [http://localhost:8090](http://localhost:8090).

2. Click `Projects` at the top.

3. Click `Add Project` from the top-right.

4. Set the name to `NetWebApplication` -- that's what we called it in the build file.

5. Click `Define your deployment process` on the top-right.

   This deployment process is not specific to an environment (dev, test, etc). Rather it's a single process that works for all environments.

6. Click `Add step`.

7. Choose the `Deploy to IIS` step template.

8. Set the Step Name to `Deploy BuildSite`

9. Set `Targets Roles` to `Web` -- it'll deploy this site along side the other site on the same "web" servers we defined previously.

10. The Package feed is `Octopus Server (built-in)`.

11. Set the Package ID to `BuildSite` -- that's what we called it in the build file.

12. Scroll down, and let's configure more deployment properties.  Here's all the properties we'd usually set in Internet Information Services (IIS).

13. `IIS Web Site` is already selected.

14. Name the WebSite `BuildSite-` then click the icon to the right, and pick `Octoups.Environment.Name` from the list.  The full Web Site name is now `BuildSite-#{Octopus.Environment.Name}`.  This means for Dev, it'll be `BuildSite-Dev` and for Test it'll be `BuildSite-Test`.

15. Set Application Pool the same way so it reads `BuildSite-#{Octopus.Environment.Name}`

16. Set `.NET CLR version` to `No Managed Code`.

17. Scroll down to Bindings, and delete the current "Port 80" binding by clicking the grey `X` on the right.

18. Click `Add binding`.

19. Change the port to `801` then pick `Octopus.Environment.SortOrder` from the variable list.  The final port is `801#{Octopus.Environment.SortOrder}`.  This means Dev will be port 8010, and Test will be port 8011.  As long as we have less than 10 environments, this strategy works great.

    In a real-world environment, we'd likely install tentacles on different machines, leave the bindings at port 80 and 443, set a host name in the bindings, and configure certificate details.

20. Click `Save` to complete the binding setup.

21. Turn on `Enable Anonymous authentication`.

22. Turn off `Enable Windows authentication`.

23. Scroll through other settings, adjusting to taste.

24. Note the `Web.<EnvironmentName>.config` section.  If we had `Web.Dev.config` and `Web.Test.config` files in the source, we could automatically transform these settings as part of the deployment.  We could also transform `appSettings.<EnvironmentName>.json` in Additional Transforms.

25. Click `Configure features` and note that it also replaces the `#{Octopus...` variables in config files as well.

26. Click `Save` on the top-right.

Note: There are code changes we should make to ensure .NET Core security features work as expected with Octopus Deploy.  See https://octopus.com/docs/deployment-examples/asp.net-core-web-application-deployments  For this workshop we'll ignore it.


Test it out
-----------

Now that we've got deployment configured in the build script, the TeamCity build defined, and the Octopus project configured, let's put it to work.  Let's commit the build script and kick off a deployment.

1. Open the TeamCity portal at http://localhost:8080 and login if necessary.

2. In the `.NET Build Script` build, click `Run`.

3. If the build went red the first time, that's ok.  Open up the failed build, switch to the Build Log tab, scroll down, maybe opening up some of the `>` buttons, and look at the gritty details.  Adjust, commit, push, and watch the build.

4. Open up the [Octopus Deploy Web Dashboard](http://localhost:8090) to see the release pushed to Dev.

5. Open up Internet Information Services, and see the `BuildSite-Dev` website and app pool.

6. Open up the Dev Website at [http://localhost:8010](http://localhost:8010) to see the deployed website.

7. This is a serious milestone.  Jump up and celebrate.  You just built your second DevOps build pipeline!
