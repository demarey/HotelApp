HotelApp
========

#Information
Source code of the Java example available at http://www.javaworld.com/article/2072203/build-ci-sdlc/an-introduction-to-maven-2.html .

![HotelWebApp model](http://images.techhive.com/images/idge/imported/article/jvw/2005/12/jw-1205-maven3-100156415-orig.gif)

This example uses Maven as build tool.

To run the example, go into the HotelWebApp module and type :
```bash
$ mvn jetty:run
```
The web application will be deployed on http://localhost:8080

#	Create your own repository 
In order to simulate developments made by a team, you will fork an existing repository on GitHub (demarey/HotelApp) .

On your computer, you will get a working copy from the forked repository to be able to work on the Hotels code.
```bash
$ git clone git@github.com:yourGitHubAccount/HotelApp.git hotels
```

#Set up the Continuous Integration infrastructure
* [Travis CI](./travis-ci.md)
* [Jenkins](./jenkins.md)

#Some development
##Add a new Hotel
In the `HotelModel` class, add a new Hotel  named « Hotel Cigogne », located at « Grand Place », with an empty town and two stars.
```java
new Hotel("Hotel Cigogne","Grand place","",2)
```
You are in a hurry and you do not want to loose time: you commit without test it before! Bad idea, but let us see...
```bash
$ git commit –m « adding a new hotel » 
$ git push
```
Now check if a build is triggered on the CI server. What is the result?
##Fix a problem
As you can see, there is a problem in a test.

**Exercise**: Find the test in error from the Jenkins server. 
coucou
coucou2
