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
By default, Docker listens to a Unix socket and not a TCP port. To change this default behaviour (needed to work with Java and the Docker Jenkins plugin), you need to provide Docker extra options:
```DOCKER_OPTS="-H=tcp://127.0.0.1:4243"```.
They can be provided by editing the Docker configuration file (/etc/default/docker) but it is only used for file is only used for upstart and SysVinit. Ubuntu 16.04 LTS uses Systemd. A quick way to add these options to Docker is to edit the Docker unit file located in `/lib/systemd/system/docker.service` and append the options to the `ExecStart` directive.
```-H=tcp://127.0.0.1:4243```

Don’t forget to restart docker to get the previous update applied: 
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

Note:
With the previous change, you will need to give the `-H=tcp://127.0.0.1:4243` option to the docker command.
Example: 
```bash
sudo docker -H=tcp://127.0.0.1:4243 run -i -t jenkins-slave /bin/bash
```
To avoid that, it is possible to define the DOCKER_HOST environment variable: `export DOCKER_HOST=tcp://127.0.0.1:4243`
