# Installation
Download Jenkins from https://jenkins.io.

Run Jenkins on http://localhost:8080
```bash
java -jar jenkins.war
```
You can now open a web browser on http://localhost:8080.

Now, install standard plugins and follow the installation wizard.

# Configuration
You can configure Jenkins by following the link in the left menu. 
#	Maven configuration
We will use the automated installer even if you already have Maven installed!
It will allow jenkins to install libraries on remote machines you will use to run jobs.
Configuration is done through Manage Jenkins/Tools global configuration
#	Email server (SMTP)
This is mandatory to get emails from Jenkins. Configure it through Manage Jenkins/System configuration/Email notification
The SMTP server should be set to a valid SMTP server (e.g. gmail or enterprise/university server). Test the service.

## Add a jenkins plugin
A great thing with Jenkins is that it is easily expandable. So, you can easily write a Jenkins plugin. That’s why there are more than 900 plugins available!

***Green balls***

Simple (and intuitive) things are often the best. A common semantic is to use the green color when all is fine and the red color when it goes wrong. 

# Create a new job
Create a new job (link in the left menu) named Hotel. 
* Choose Build a free style job.
* Write a small description of the job and set the number of builds to keep to 2.

You now need to define where to find the code (the forked GitHub repository).

**WARNING** : to avoid to declare SSH keys, use the https protocol for the project URL.

A good strategy for code retrieving is to emulate a clean checkout to reduce the bandwidth consumption.


Continuous Integration is useful if tests are run after each code integration (a commit on the source code repository). Simply, there are 2 ways to obtain this result:
* Poll the SCM (for example every 5 minutes). But this approach implies an overload of the SCM server and is not very efficient.
* The best way is to define a post-hook commit that will warn Jenkins of a new commit and trigger a new build.
With the GitHub plugin, it is easier: you can simply check the box *GitHub hook trigger for GitSCM polling*

Add post-build actions:
* to get an email notification
* to publish Junit test report. You can use ```**/surefire-reports/*.xml```for the location of XML reports.

Endly, configure the build notification : your email adress (should be the developers mailing-list in a « real » situation).

# Run a job
Your job is now configured! Let us go to see if it works! 

Simply click on the run icon beside the job on the dashboard.

Then, to visualize the result or the current state of a job, open the console associated to this build: Click on the job, then on the build number you want to visualize, endly on the console link  in the left menu.

Check the log output and catch following steps:
* Automated Maven installation by Jenkins (for the first run)
* Code checkout
* Maven modules discovery
* Build, tests.
If the build succeeds, you will see a green bullet on the dashboard.

# Some development
Follow instructions on README.md

# Code coverage
Install the Cobertura plugin from Jenkins.
Set the job build goal to :
```
cobertura:cobertura
```
Activate Cobertura coverage report publication in the job configuration and set the cobertura xml report pattern to:
```
**/target/site/cobertura/coverage.xml
```
# C++ build with Jenkins
##	CMake plugin installation
Install the CMake plugin from Jenkins.
## Configure a new job
We will use an already defined C++ project.
* Create a new job (free-syle project) named cpp-project.
* Configure the source code repository (subversion) to:
```
https://scm.gforge.inria.fr/anonscm/svn/ecoleadt11/trunk/practicalClass/tests/C++
```
Then, add a CMake build step. Finally, trigger a build manually and check the output

# Bonus
* Install a Jenkins server on a virtual machine provided by the university (OpenStack)
* [Use docker with Jenkins](./jenkins-docker.md)
