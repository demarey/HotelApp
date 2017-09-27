# Use Docker containers to run your Jenkins jobs
Here is a small recipe you can use to run Jenkins jobs in Docker containers.

[Docker](https://www.docker.com/) is a solution to easily automate the deployment and execution of applications inside software containers on top of Linux. The main advantages to use containers are the isolation of resources, restriction of services, and processes provisioned to have a private view of the operating system. To summarize, containers helps to isolate a process and its dependencies. With container, it is also very easy (and cheaper than virtual machines) to start from the same initial container state for each run. It is a very good property for Continuous Integration to have reproducible builds without side effects.

## Install Docker
The best way to install Docker is to follow [Docker documentation](https://docs.docker.com/engine/installation/). For this tutorial, we will use an Ubuntu 16.04 LTS distribution. We will the follow [Docker installation instructions for Ubuntu](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-using-the-repository), more specifically the section explaining how to install Docker from a repository.
### Testing Docker installation
To check that the installation succeeded, run
```bash
sudo docker run -i -t ubuntu /bin/bash
```
### Docker condifuration
By default, Docker listens to a Unix socket and not a TCP port. To change this default behaviour (needed to work with Java and the Docker Jenkins plugin), you need to provide Docker extra options: `DOCKER_OPTS="-H=tcp://127.0.0.1:4243"`.
They can be provided by editing the Docker configuration file (/etc/default/docker) but it is only used for file is only used for upstart and SysVinit. Ubuntu 16.04 LTS uses Systemd. A quick way to add these options to Docker is to edit the Docker unit file located in `/lib/systemd/system/docker.service` and append the option `-H=tcp://127.0.0.1:4243` to the `ExecStart` directive.

Don’t forget to restart docker to get the previous update applied: 
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

**Note:**
With the previous change, you will need to give the `-H=tcp://127.0.0.1:4243` option to the docker command.
Example: 
```bash
sudo docker -H=tcp://127.0.0.1:4243 run -i -t jenkins-slave /bin/bash
```
To avoid that, it is possible to define the DOCKER_HOST environment variable: 
```bash
export DOCKER_HOST=tcp://127.0.0.1:4243
```

## Create a Docker image to be used by Jenkins slaves
A Docker image is a template that is used to create new containers. It includes everything needed to run the process you want (here a Jenkins job) inside the container.

The way do that is to create a Dockerfile with all instructions needed to build an image.
```bash
cat > Dockerfile
# This Dockerfile is used to build an image containing basic stuff to be used as a Jenkins slave build node.

FROM ubuntu:latest
MAINTAINER Christophe Demarey

# Make sure the package repository is up to date.
# RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update

# Install a basic SSH server
RUN apt-get install -y openssh-server
RUN mkdir -p /var/run/sshd

# Install JDK 7 (latest edition)
RUN apt-get install -y --no-install-recommends openjdk-7-jdk

# Install packages needed for the build
RUN apt-get install -y --no-install-recommends git maven

# Add user jenkins to the image
RUN adduser --quiet jenkins
# Set password for the jenkins user (you may want to alter this).
RUN echo "jenkins:jenkins" | chpasswd

# Standard SSH port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
```
The given script will produce a new Docker image from a basic Ubuntu image, by adding SSH, JDK, git and maven, as well as a new user jenkins with password jenkins. It also set the default command to execute: sshd.
If the build needs more system dependencies, they should be added in this file.

To generate the Docker image, run `sudo docker build -t jenkins-slave .` in the directory containing the Dockerfile.

## Configure Jenkins
### Configure global security
Disable builds on the master: *Manage Jenkins / Manage Nodes / Master / Configure* and set the number of executors to `0`.
### Docker Plugin
Install the Docker plugin from the plugin manager (*Manage Jenkins / Manage plugins*).
The, configure the Docker plugin (*Manage Jenkins / Configure System / Docker*) with following information:
* name: `local docker`
* url: `tcp://127.0.0.1:4243`
* image
  * id: `jenkins-slave`
  * credentials: `jenkins/jenkins`
  * remote fs: `/home/jenkins`

We gave information to Jenkins so that he knows:
* how to contact the docker instance,
* which Docker images are usable from Jenkins (we just declared one image).

### Configure your job to use Docker
The last part you need to run jobs on Docker containers is to configure the job.
Here, some magic happens: you just need to tell your job to use a Docker image and Jenkins will magically create a new Docker container on-demand (i.e. when the job is triggered) from the specified Docker image.
To specify the Docker image to use, put the image id in the Restrict where this project can be run field of the job configuration.
