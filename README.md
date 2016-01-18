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
[Travis CI](./travis-ci.md)
[Jenkins](./jenkins.md)

