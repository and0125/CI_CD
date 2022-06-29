# CI_CD

Notes and projects involving continuous integration and delivery

## Chapt. 1

Continuous delivery is the ability to get changes of all types into production in a safe, quick, and sustainable way.

### Traditional Delivery

All delivery processes start with the requirements defined by the customer and ends with the production release. The differences between the tradtional and continuous delivery process are in between those two points.

In traditional releases, the release cycle starts with the requirements provided by the product owner, who represents the customer. Then there are three phases in which the work is passed to several teams:

- Development team: The developers implement the solution, and use some agile techniques to increase the speed of development and improve communication with the client.
- Quality Assurance team: this phase is usally called user acceptance testing, and it requires a code freeze on the trunk code base, so that no new development breaks the tests. The QA team performs a suite of integration testing, acceptance testing, and non-functional analysis (things like performance testing, recovery, security, and so on). Any bug detected goes back to the developers.
- Operations, the final phase, means passing the code to the operations team to release the code and monitor the production system. If anything goes wrong, they pass the info to the develoeprs to work on.

This traditional delivery can take anywhere from a week to a few months, possibly even a year. It has some typical drawbacks:

- slow delivery: the customer receives the product long after the requiremenst were specified.
  long feedback cycle: the feedback cycle for bugs and user feedback happens well after the developers finish.
- lack of automation: rare releases don't encourage automation, which leads to unpredictable releases.
- risky hotfixes: hotfixes can't wait for the full UAT phase, so they tend to be tested less or not at all.
- Stress: unpredictable releases are stressful for the operations team.
- Poor communication: work passed from one team to another represents the waterfall approach, in which people start to care only about their part, rather than the complete product. Errors end in finger pointing rather than collective solutions.
- Shared responsibility: no team takes responsibility for the product from soup to nuts.
  - for developers done means that requirements are implemented.
  - for testers done means the code is tested
  - for operations done means the code is released
- Lower job satisfaction: each phase is interesting for the main team, but other teams need to support the process, which is usually not that interesting to the other groups.

### Continuous Delivery

The question to ask for deliver is: 'how long will it take for your organization to deploy a change that involves a single line of code?'. This is how to measure the health of your delivery process.
To be able to deliver continously, we need automation. That is why, CD is about changing each phase of the traditional delivery process into a sequence of scripts, called the automated deployment pipeline, or the CD pipeline.

A CD pipeline brings:

- faster deliveries
- fast feedback cycles
- low-risk releases: if you release daily, the process becomes repeatable and safer
- flexible release options: everything is prepped if you need to release immediately.

### Implementing CD Pipeline

An automeated deployment pipeline is a sequence of scripts that is executed after every code change is committed to the repository. If the process is successful, it ends up with deployment to the production environment.

Each step corresponds to a step in the traditional delivery process:

- Continuous Integration: this checks to make sure that the code written by different developers is integrated.
- Automated Acceptance Testing: this checks if the client's requirements are met by the develoeprs implementing the features. This testing also replaces the manual QA phase.
- Configuration Management: this replaces the manual operatiosn phase, it configures the environment and deploys the software.

#### Continuous Integration

This phase provides the first feedback to develoeprs. It checks the code from the repository, compiles it, runs unit tests, and verifies the code quality. If any step fails, the pipeline execution is stopped, and the developers should fix the CI build. It must be executed in a timely manner, so that developers dont build on faulty code in between deployments. The CI pipeline is solely the domain of the developers, it doesn't require agreement from QA and the operations teams.

#### Automated Acceptance Testing

This is a suite of tests written together with the client and QA testers that is supposed to replace the manual User Acceptance Testing stage. It acts as a quality gate for whether or not the code is ready to release.If any acceptance tests fail, pipeline execution is stopped and no further steps are run. This prevents moving into the configuration phase and stops the release.

The idea of this stage is to build quality into the product rather than verifying it later. This phase is usually the hardest to develop because it requires close cooperation between the clients and the QA team, and creating tests at the beginning instead of at the end of the deployment process. this is particularly difficult for legacy systems.

There are four types of testing to be done in Automated Acceptance Testing, three of which are actually automated:

- Acceptance testing: these tests represent the functional requirements seen from the business perspective. They are written in the form of stories or examples by clients. The developers and the clients must come to agreement on how the software should work.
- Unit testing: these are test that help the developers to provide high quality software and minimize the number of bugs.
- Exploratory testing (manual): this is the manual black-box testing, which tries to break or improve the system. Manual QAs continune to perform exploratory testing, try to play with hte system, and try to break it, then think about improvements.
- Non-functional testing: these are tests that represent the system properites related to performance, scalability, security, and so on.

In an automated CD process, there is no place for manual QAs that perform repetitive tasks.

Integration tests are a special set of Automated Acceptance Testing, and the meaning differs depending on the context and the system. For microservices, integration tests may only consist of unit tests and acceptance tests, because the services are small, and if they work independently, they work together. If you build a modular applpication (services that consists of multiple nodes) then there is a need to test multiple modules (but not the whole application) and those would be considered integration tests.

For this second case, modular applications, the integratrion tests are usually very technical and require mocking external services and internal modules to successsfully test the binding of the multiple modules.

##### Testing Pyramid

The testing pyramid, from bottom to top is:

- unit tests
- integration tests
- acceptance tests

As you move down the hierarchy, the tests become slower and more expensive to create. They often require user intefaces to be touched and a separate test automation team to be hired. That is why acceptance tests should not target 100% coverage.

Acceptance Testing should be feature-oriented, and verify only selected test scenarios.

Unit tests should be cheap and fast, so developers should strive for 100% coverage of these. providing them is a standard for any mature team.

#### Configuration Management

This phase is responsible for tracking and contrlling changes in the software and its environment. It involves taking care of preparing and installing the necessary tools, scaling the number of service instances and their distribution, infrastructure inventory, and all tasks related to application deployment.

#### Pre-requisites For Continuous Development

A successful CD pipeline depends on tools but also on:

- your organizations structure and its impact on the development process
- Your products and their technical details
- your development team and the practices you adopt

##### DevOps Culture

A DevOps culture means a single person or a team is responsible for development, Quality Assurance, and Operations.

Functional requirements in this culture have to be separated into services or modules, so that each team member can take care of an independent part.

In this culture, the role of the client/product owner changes because there is no user acceptance testing, the client becomes a big part of writing acceptance tests. This means the client has to become more technically-oriented.

In agile teams, its a common thing to reject user stories/requirements without acceptance tests attached. This technique, even though its strict, often leads to better development productivity.

The business decisions of the organization must change to work with the new release timelines, nad manual parts of the pipeline must be kept in consideration to meet some of the enterprise goals. Continuous Deployment can be utilized to automate code pushes all the way to production, whereas Continuous Delivery means that releasing to production is a manual step, which can be used to speed up the deployment timeline.

#### Technical Prerequisites

- automated build, test, package, and deploy operations
- quick pipeline execution
- quick failure recovery: the possibility of a quick rollback or system recovery must be included.
- Zero-downtime deployment: the deployment cannot have any downtime since we release many times a day.
- Trunk-based development: Developers must check-in code regularly into one master branch, otherwise, if everyone develops in their own branches, integration is rare, therefore releases are rare, which is the exact opposite effect of what we want to achieve.

#### Tools

The tool is less important than understanding its role in the process.

From this book, Jenkins can be replaced with Atlassian Bamboo, and Chef can be used instaed of Ansible.

##### Docker Eco-System

Docker allows developers to package an appplciation in the environment-agnostic container image and therefore treats servers as a farm of resources, rather than as machines htat must be configured for each application.

Docker has a number of supporting technology to understand as well:

- Docker Hub: registry for docker images
- Kubernetes: this is a container orchestration software

##### Jenkins

This is the most popular automation server. It helps create CI and CD pipelines, and can, in general, automate any sequence of scripts. It has a great community that extends it with new features as well. This allows us to write the pipeline as code and supports distributed build environments.

##### Ansible

Ansible is an automation tool that helps with software provisioning, configuration management, and application deployment. It will soon overtake Chef and Puppet. It uses an agentless architecture and integrates smoothly with Docker.

##### GitHub

best known hosted version control system.

##### Java/ Spring Boot/ Gradle

Java has been the most popular programming language for years. This is what's used in the book to create a simple web service. Gradle is a build tool, it's less popular than Maven, but trending upwards. Any programming lnaguage can be exchanged and the CD process will be the same though.

##### Other Tools

Cucmber was chosen as an acceptance testing framework. Similar tools are FitNesse and JBehave. For database migration, you can use Flyway, but any other tool would do, like Liquibase.

## Chapt. 2: Docker

NOTE: I read through most of the docker chapter without taking notes because it seemed straightforward. 

### Docker Images and Containers 

a docker image is a stateless building block, which has a collection of all the files necessary to run your application together wiht the recipe on how to run it THe image is stateless, so you can send it over the network, store it in the registry, name it, version it, and save it as a file. 

A container is a running instance of an image. We can create many containers from the same image if we want to have many instances of the same application. Since containers are stateful, this means we can interact with them and make changes to their states. Let's look at an example of a container and image layers structure. 

The bottom layer always contains the base image, which represents an operating system and the image is built on top of the existing base image. It's possible to create a new base image, but this is rarely needed. Images can be used to add toolkits (like Git, Python, Java, etc) which enable the user to use those programs without installing any tools on their local machine. 

The images are stored locally and new containers can pull from the same images and base images without downloading the image from a registry. 

You can use the docker command line interface to search for necessary docker images by using the search command:

```
$ docker search mongo
```

Note that the search results returned without any prefixes are from the dockerhub, and as such is assured to be stable and maintained. Other images are available and are likely open source projects that can be researched and used as well. 

The dockerhub stores more than 100k images. 

### Building images

The power of docker comes from being able to wrap programs together with the environment in new images. 

This can be done using the Docker commit command or using a dockerfile automated build. 

#### Docker Commit Command

containers are stateful and writable, so once one container is running, you can install programs from the internet via the command line.

Once you have a container started and have changed the image, you can then  run the diff command to see how the container is different from the image:

```
docker diff <container id>
```

Then commit the container as an image:

```
docker commit <container id> new_name
```

This will create a new docker image with the base image and the new files/packages installed.

#### Dockerfile

Creating each image manually can be extra work. Docker ships with a built-in language to specify all the instructions that should be executed to build the Docker image, called a Dockerfile.

Example Dockerfile:

```dockerfile
FROM ubuntu:18.04

RUN apt-get update && apt-get install -y python
```

After creating this docker file, you can run:

```docker
docker build -t image_name DIR
```

(where DIR is the directory of the dockerfile)

Common docker commands include:

FROM: defines the image on top of which a new image will be built
RUN: specifies the commmands to run inside the container
COPY: copies a file or a directory into the file system of the image
ENTRYPOINT: define which application should be run in the executable container

You can now run the image as a container, and even pass in an environment variable via the command line:

```docker
docker run -e NAME=Aaron image_name
```

Alternatively, an environment variable can be specified in the dockerfile:

```dockerfile
ENV NAME Aaron
```

In a situation where both the dockerfile and the command line specify the same environment variable, the command line takes precedence.

To run a container in the background, use the -d flag:

```docker
docker run -d -t ubuntu:18.04
```

When run in the background, you can access the logs of a container with the command:

```
docker logs <container id>
```

Containers are always in one of four states:

- Running
- Paused
- Stopped/Exited
- Restarting

### Docker Networking 

most applications don't run in isolation, they need to communicate with other systems over the network. If we want to run a website, we need to first understand how to run a service and expose its port to other appplciations. 

To make a program accessible thats running in a docker container, you need to expose a port. This process is called port mapping - it maps a port from the container to a port on the host machine. This port mapping is performed with the publish command:

```
docker run -d -p 8080:8080 image_name
```

This port mapping allows you to expose ports from various docker containers and run them as microservices, using their ports for communication. 

### Container Networks

The docker0 port is the way the docker container can access the internet without interfering with the host machine.  It is created by the Docker Daemon to connect with the Docker container. 

You can use the docker inspect command to examine the interfaces of a container:

```
docker inspect <container id>
```

This will specify the IP address the container is using. YOu can see its an IP address similar to that of the Docker host.  Even with the ability to connect via this IP address, it's a best practice to publish a port to establish the communication between the container and the host machine or other containers. 

You can do this by specifying a network, because, by default, Docker containers don't open any routes from exeternal systems.  This can be changed by using the network flag.

The default network is called the 'bridge' which is Docker's default bridge, and is also the most popular network option because it lets you define explicitly which ports hsould be published and is both secure and accessible. 

It can happen that you run two containers on the same port, in which case docker will throw an error. When this occurs, you can either specify another port, or you can let docker find the next available port for you. 

- docker run -p : publishes the container port to the unused host port (one port)
- docker run -P: publishes all ports exposed by the container to the unused host ports. (all ports)

### Using Docker Volumes

Docker volume is the docker host's directory mounted inside the container. It allows the container to write to the host's filesystem as if it was writing to its own. This enables the persistence and sharing of the container's data. 

The volume can be specified either at the command line or in the docker file:

- command line: docker run -i -t -v ~/docker_ubuntu:/host_directory ubuntu:18.04 /bin/bash
- dockerfile: VOLUME /host_directory

If the docker container has a volume specified by is run without the -v flay specified, the container's host directory will be mapped to the host's default directory for volumes /var/lib/docker/vfs/.  This is a good solution if you deliver an applcation as an image and you know it requires permanent storage. 

A common approach to data management with docker is to introduce an additional layer of data volume containers. These are containers that are only meant to declare the volume for data storage and nothing else.  Then, other containers can use it instead of declaring a volume directly. 

### Using Names in Docker

in many cases it can be better to give a friendly, easily recognisable name to the container or image. This can be helpful for convenience but also for automation.

To name a container from the command line use:

```
docker run -d --name tomcat tomcat
```

When a container is named, it still has its auto-generated hash ID. Both the name and the ID are unique and can be used to reference the container.

### Tagging Images

the -t flag is used to tag images:

```
docker build -t hello_world_python .
```

If this command isn't used, the image will be built without any tags, and would need its hash ID specified in order to run it. 

An image can have mutliple tags, but these should follow the following format:

```
<registry_address>/<image_name>:<version>
```

Where:

- registry_address: IP and port of the registry or alias name
- image_name: name of the image that is built
- version: A version of the image in any form

### Cleaning Up Docker Containers

To see all docker containers:

```
docker ps -a
```

To delete a stopped container:

```
docker rm <ID or name of container>
```

to remove all stopped containers:

```
docker rm (docker ps --no-trunc -aq)
```

the -aq means to pass only iDs for all containers; and the --no-trunc flag asks docker not to truncate the output of the ps command. 

to specify a container should remove itself after being stopped:

```
docker run --rm <ID or name of container>
```

In most applications, stopped containers are left only for debugging purposes. 

### Cleaning up docker Images

cleaning up images can easily cause a no space left on Device error. 

To see all images, use the command:

```
docker images
```

remove image by name or ID:

```
docker rmi <ID or name of container>
```

The automated cleanup for images is more involved because images don't have a state, so its not possible to remove then because they aren't in use. The common strategy is to set up a cron cleanup job containing the following command:

```
docker rmi (docker images -q)
```

To prevent the removal of images with tags (to not remove all the latest images) you can use the dangling parameter:

```
docker rmi docker images -f "dangling=true" -q
```

And if those containers have volumes:

```
docker volume ls -qf "dangling=true" | xargs -r docker volume rm
```

## Chapt. 3: Jenkins

Jenkins is an automation server, that can get tricky to configure over time as the amount of tasks assigned to it increase over time.  Docker allows for dynamic provisioning of Jenkins agents, and its worth time to configure everything correctly upfront, so that things scale properly when needed.

Jenkins is an open source automation server  written in Java, and it is the most populare tool for CI CD processes. It's highly valued for its simplicity, flexibility, and versatility. 

Benefits of Jenkins include:

- plugins that support ost programming languages and frameworks. And since it can use any command and any software, it is sutiable for every automation process imaginable.
- it can run on any operating system because its written in Java, and can be delivered in severa different versions.
- Supports most source code management and build tools. there is no other continuous integration tool that supports so any external systems.
- Jenkins has a built-in mechanism for handling manager and worker nodes which distributes execution across multiple nodes, located on multiple machines. It can also distribute amongst nodes with different operating systems installed.
- The installation and configuration process itself is simple. There is no need to configure any additional software or database for Jenkins to run.
- Jenkins pipelines are defined as code, and Jenkins can be configured using XML or Groovy scripts.  This allows for keeping the configuration in the source code repository and helps in the automation of the Jenkins configuration.


There are many ways to install Jenkins, one way being a docker-based method. 

### Installing Jenkins on Docker

The jenkins image is available from Docker hub:

```
docker run -p <host_port>:8080 -v <host_volume>:/var/jenkins_home jenkins/jenkins:2.150.3
```

The parameters for this command are:

- host_port: the port under which Jenkins is visible outside of the container
- host_volume: the directory where the jenkins home is mapped. It needs to be specified as a volume so that it persists permanently, because it contains the configuration, pipeline builds, and logs. 

To check whether Jenkins is running, you can print the logs: 

```
docker logs jenkins
```

In the production environment, you may also want to set up the reverse proxy in order to hide the jenkins infrastructure behind the proxy server.  A short description of how to do this using the NGINX server can be found (here)[https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+with+Docker]

Without docker, you can install jenkins directly, but must have Java 8 or higher already installed (see book).

The installation guides can be found (here)[https://jenkins.io/doc/book/installing/]

### Initial configuration Locally

Jenkins can be open in the browser at its localhost IP address. It will require an admin password, which is created in the Jenkins logs, which can be accessed using 'docker logs jenkins'.

Jenkins will ask to install several plugins, and you may as well install them all initially. 

After the plugin installation, you will setup a username, password, and other info for an account. If you skip this part, the token generated in the logs will be used as the admin password. 

### Configuration in the Cloud

There are some services to install Jenkins in the cloud for you, like CloudBees, but this has a subscription.  This offers the advantage you don't have to maintain Jenkins instances, which can be helpful for small companies. 

### First Pipeline

Once you've gotten to the browser and logged in, you can click new item to start a new pipeline, call it hello world.

There are a lot of options including freestyle projects and multi-configuration project, for projects that need a number of configurations.

Once in the pipeline window there are several tabs of options for pipelines, but in the advanced project options, you can write a script to define the pipeline. 

See book for hello world code (pg 83). Once that's written in the advanced pipeline options, you can click build now. 

If you cluck on the build history, then on Console output, you can see the log from the pipeline build. 

### Jenkins Architecture

Jenkins pipelines can spend time downloading files, compiling code, running tests, and can be used by the whole team or a whole organization. 

Unless the project is really small, Jenkins should not execute builds at all, and delegate the builds to worker nodes.  This is called running Jenkins 'master' (or manager). The manager delegates tasks to workers or agent nodes.

In a distributed environment, Jenkins manager is responsible for:

- recieving build triggers
- sending notifiations 
- handling HTTP requests
- Managing the build environment (orchestrating the jobs that execute on agents)

Managers require different computing environments than worker nodes:

- Managers require a dedicated machine with RAM ranging from 200 MB to 70+ GB for huge, single-manager projects.
- Workers: there are no genreal requirements for workers except that they should be capable of executing a single build, which depends on the size of the project. If the project requires 100GB of RAM, the worker nodes need to at least meet this requirement. 

These workers/agents should be as generic as possible, so that jobs in any programming language can be run on them. If agents cant' be generic enough to match all projects, then its possible to label some agents and projects, so that the given build will only be executed on the properly labelled agents. 


This relationship between managers and workers happens through the following steps:


### Scaling 

Vertical scaling: means that as the manager workload grows, more resource are applied to the manager machine. Keeping one manager can be useful because when updatig Jenkins, it only has to happen in one place. 

Horizontal scaling: as an organization grows more manager instances are launched.  This requirers a smart allocation of manager instances to teams, and potentially enable each team to have its own manager.

The drawback is that it may be difficult to automate cross-project integrations, and that a part of the team's development time is spent on the Jenkins maintenance.  But this horizontal scaling has a few huge benefits:

- manager machines don't need to be expensive computing servers
- difference teams can have different Jenkins settings
- teams usually feel better and work with jenkins more efficiently if the manager is their own
- If one manager goes down, it does not impact the whole organization
- The infrastructure can be segragated into standard and mission-critical components

### Multi-Jenkin Instances

In order to test Jenkins upgrades, new plugins, and new pipeline definitions, its best to have two Jenkins instances available: Test and Production. 

The test environment should be as similar as possible to the production environment and so should have a similar number of agents attached.

### Setting up Agents

In order for a manager to communicate with an agent, the bi-directional connection has to be created. 

There are different options for this, two of which are:

- SSH: secure shell. Jenkins has an SSH client built in, so the only requriement is that the SSH daemon server is installed and configured on the workers. This is the most convenient and stable method because it uses standard Unix mechanisms.
- Java web start: a java application is started on each agent machine and the TCP connection is established between the Jenkins worker and the jenkins manager. This method is useful when the workers are inside a fire-walled network and the manager cant initiate the connection directly. 

At a low level, agents always commmunicate with the Jenkisn master using one of these protocols. The difference between them is because of two aspects:

- static versus dynamic: the simplest option is to add workers permanently to the Jenkins manager. The drawback is that it requires a manual change every time more or fewer worker nodes are needed. A better option is to dynamically provision these workers.
- Specific vs. general purpose: agents can be specific, or general purpose. 

These result in four common strategies for how agents are configured:

- permanent agents: simplest option. Can be done through the GUI (see book, pg 90). 
  - using this technique through the GUI, you can set the number of executors to 0 on the manager node to ensure it's not used for jobs as well. 
  The drawback is that these agents have to be maintained separately if you have multiple types of projects. This is same issue with maintaining multiple production server types. 
- permanent docker agents: the idea behind this solution is permanently add general purpose agents that can handle multiple types of jobs. Then no labels are needed, because all the agents can be the same. The configuration is static, so it's done exactly the same way as the permanent agents, except for the fact that docker is needed on each agent used as a worker. Then, when the build is started the Jenkins worker start a container from theh docker image, and then executes all the pipeline stesp inside the container. This way, the execution environment is consistent and its not required to configure each worker node separately. 
- Jenkins swarm agents: This is similar to the permanent docker agents solution, except that it Jenkins swarm allows you to dynamically add worker nodes without needing to configure them in the Jenkins manager explicitly. 
  - first step to use the self-organizing swarm plug-in modules in Jenkins.  This is done through the manage Jenkins>Manage Plugins menu. 
  - Then:
    - open the manage Jenkins page
    - Go to configure system
    - In the cloud section, click add a new cloud, and choose docker. Follow book (pg 95+) for more information.
    - There are common worker images, but you can also build your own worker images to use.
- dynamically provisioned docker agents: thes can be treated as a layer over the standard mechanism. 
  - when the jenkins job is started, the manager runs a new container from the worker image on the worker's docker host.
  - The worker contaienr is actually the ubentu image (or another).
  The jenkins manager automatically adds the create worker to the list of workers.
  - The agent is accessed using the SSH communication protocol to perform a build of the pipeline.
  - After the build the manager stops and removes the worker container. 
  - This ends up in a process similar to the permanent docker agents solution, but the difference is the new workers are dockerized and can be build from an image stored on each of the workers. This enables:
    - The process of creating, adding, and removing agents can be automated.
    - the worker Docker host can be more than just one machine, and can be a cluster composed of multiple machines. In such a case, adding more resources is as simple as adding a new machine to the cluster, and doesn't require any changes in Jenkins.
    

## Continuous Integration Pipeline

A pipeline is a sequence of automated operations that usually represent a part of the software delivery and quality assurance process. It can be seen as a chain of scripts that provide the following additional benefits:

- Operation grouping: operations are grouped toegether into stages (also know as gates or quality gates) That introduce a structure into the process and clearly define the rule. If one stage fails, no further stages are executed.
- Visibility: All aspects of the process are visualized, which helps in quikc failure analysis, and promots team collaboration.
- feedback: team members learn about problems as soon as they occur, so that they can react quickly. 

### Pipelines in jenkins

a pipeline consists of two kinds of elements: stages and steps. Each stage can have multiple steps.

- Step: a single operation that tells jenkins what to do.
- stage: a logical separation of steps that groups conceptually distinct sequences of steps. For example, the Build, Test, and Deploy stages are used to visualize the Jenkins pipeline progress. 

It is possible to create steps that run in parallel, but its better not to do this unless its needed to optimize the build time. 

### Pipeline Syntax

A jenkins pipeline script uses a declarative syntax that is recommended for all new projects. The Othe options are Groovy-based DSL and potentially XML for older versions of Jenkins.

This declarative syntax is was designed to make it as simple as possible to understand the pipeline, even by the people who do not write code on a daily basis. So the syntax is limited to only the most important keywords.

See page 117 for the example script, but this contains all the possible jenkins keywords:

- use any available agent: `agent any`
- execute automatically every minute: `triggers { cron('*****')}`
- stop of the execution takes more than five minutes: `options { timeout(time:5)}`
- Ask for a boolean input parameter before starting: see book
- Set Rafal as the NAME environment variable
- Only in the case of the true input parameter:
  - print Hello from Rafal
  - print testing the chrome browser
  - print testing the firefox browser
- Print `I will always say hello again` regardless of whether there are any errors in the execution.

### Important Keywords 


- Sections: sections define the pipeline structure and usually contain one or more stage directives. These use the following keywords:
  - stages: defines a series of one or more stage directives
  - Steps: defines a series of one or more step instructions
  - post: defines a series of stesp that are run at the end of hte pipeline build. They are marked with a condition (either always, success, or failure) and usually are used to send notifications after the pieline build.
- Directives: Directives express the configuration of a pipeline or its parts. 
  - Agent: This specifies where the execution takes place and can define the label to match the equalliy-labelled agents, or docker to specify a container that is dynamcially provisioned to provide an environment for the pipeline execution. 
  - Triggers: This defines automated ways to trigger the pipeline and can use cron jobs to set the time-based scheduling, or pollSCM to check the repository for changes.
  - Options: this specifies pipeline specific options, for example, timeout (maximum time to wait for) or retry (the number of times the pipeline should be re-run after failure).
  - Environment: This defines a set of key values used as environment variables during the build. 
  - parameters: this defines a list of user-input parameters.
  - Stage: this allows for the logical grouping of steps
  - When: this determines whether a stage should be executed depending on the given condition.

  



