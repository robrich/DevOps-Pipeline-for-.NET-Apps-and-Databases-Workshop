OCTOPUS DEPLOY TO TEST
======================

Just like we did in the other build, promoting this new project to the test environment is cake.

1. Open up the [Octopus Deploy Web Dashboard](http://localhost:8090) to see the release pushed to Dev.

2. Click on the green check under the `Dev` column for the `TodoStore` project and open up the details of this release.

3. In the top-right, click `Promote to Test`.

4. In the top-right, click the green `Deploy` button.

5. Click `Dashboard` on the top-left, and see the build deploying to test.

6. After it finishes, open up SSMS, right-click on the `TodoStoreDev` database and choose `Refresh`.

7. Click on the `[+]` buttons to see the newly deployed schema changes.
