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
