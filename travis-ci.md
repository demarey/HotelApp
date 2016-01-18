> Travis CI is a FOSS, hosted, distributed continuous integration service used to build and test projects hosted at GitHub. 
>
>Travis CI is configured by adding a file named .travis.yml, which is a YAML format text file, to the root directory of the GitHub repository.
>
>Travis CI automatically detects when a commit has been made and pushed to a GitHub repository that is using Travis CI, and each time this happens, it will try to build the project and run tests. This includes commits made to any branch, not just to the master branch. Travis CI will also build and run pull requests. When that process has completed, it will notify a developer in the way it has been configured to do so — for example, by sending an email containing the test results (showing success or failure), or by posting a message on an IRC channel.
>
>Travis CI can be configured to run the tests on a range of different machines, with different software installed (such as older versions of a programming language implementation, to test for compatibility)
>
>from Wikipedia

You can sign in with your Github account!

To know more on TravisCI, please read https://docs.travis-ci.com/user/getting-started/.

# Building your project
HotelApp is a java application using maven.

Useful information can be found at: https://docs.travis-ci.com/user/languages/java.

**Exercise**: get your project ran by Travis CI


## Basic steps
* Sign in,
* go to your profile page and enable Travis CI for the repository you want to build,
* add a `.travis.yml` file to your repository to tell Travis CI what to build
