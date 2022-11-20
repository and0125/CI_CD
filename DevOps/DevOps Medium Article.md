# Notes from DevOps Article

Using the following tech for deployment:

- Docker
- Kubernetes
- Terraform
- AWS

## Docker

sets up an isolated userspace on your computer, called a container, which you can modify. A container is created from a base image, and, once modified, you can save the updated container as another image to be deployed.

Note that you need to make sure to do port mapping to open up the isolated userspace on your machine to actually be accessible by your own machine.

## Kubernetes

This is a container orchestration tool, and is meant for running and managing applications packaged as containers across a fleet of servers.

Kubernetes has the following functionality out of the box:

- scheduling: which allows you to pick the optimal servers to run your containers.
- Deployment: allows you to roll out changes to your containers without downtime.
- Auto-healing: automatically redeploy containers that failed.
- Auto-scaling: scale the number of containers up and down based on the load.
- Networking: routing, load balancing, and service discovery for containers.
- configuration: configure data and secrets for containers.
- data storage: manage and mount data volumes in containers.

Kubernetes has two main components:

- control plane: a control plane that manages the cluster. It stores the state of the cluster, monitoring containers, and coordinating actions across the cluster. It also runs the API server ,which provides a command line utility (kubectl), a web UI (the kubernetes dashbaord), and infrastructure as code tools (terraform) which can control what happens in the cluster.
- Worker Node: the worker nodes actually run your containers. The worker nodes are managed by the control plane.

To deploy something in Kubernetes, you create kubernetes objects which are persistent entities you write to the cluster via the API server which record the application configuration you intend to have. The cluster runs a reconciliation loop which continously checks the objects you stored in it and works to make the state of the cluster match your specified intent.

There are many types of kubernetes objects, but for the tutorial, they use the following two:

- Kubernetes Deployment: this is a declarative way to manage an application in Kubernetes. You declare what images to run, how many copies of each to run, and a variety of settings for them, a strategy to roll out updates to those images, and then the Kubernetes Deployment will work to ensure these requirements are always met.
- Kubernetes Service: this is a way to expose a web app running in Kubernetes as a networked service. This can be used to configure a load balancer which exposes a public endpoint and distributes traffic from that endpoint across the replicas in a kubernetes deployment.

### Commands

To run an a cluster from the command line, you can use a command like this to create a deployment:

```bash
kubectl create deployment simple-webapp
  --image training/webapp
  --replicas=2
  --port=5000
```

breakdown:

- command: kubectl create deployment
- cluster name: simple-webapp
- docker image: training/webapp
- replicas: 2
- port: 5000

You can create a service using the following command:

```bash
kubectl create service loadbalancer simple-webapp --tcp=80:5000
```

breakdown:

- command: kubectl create service
- type: loadbalancer; tells kubernetes to deploy a load balancer to route traffice across your replicas.
- name: simple-webapp
- port: map port 80 from your localhost to port 5000 in the docker container.

You can use the `kubectl get deployments` command to view active deployments, and the `kubectl get pods` commands to view the pods (groups of containers) in each cluster.

This is an expansion from the `docker run` command because you can stand up multiple containers at once, close to the `docker-compose` command. Also, these containers are being actively monitored and managed. To check that the containers are managed, try running `docker kill` on one of the container ids, and then run `docker ps` again afterwards to see that the container is replaced almost instantaneously, and its running a load balancer to distribute traffic across those replicas.

There are commands to run from the `kubectl` CLI tool, but even this is not the best way to manage apps in production. A better option is to manage all of your infrastructure as code.

This will be done with Terraform.

You can improve the manual deployment a bit by storing the configuration of your kubernetes objects in yaml files.

### Reference Articles

Reference articles for Kubernetes:

- <https://blog.gruntwork.io/a-crash-course-on-kubernetes-a96c3891ad82>
- <https://blog.gruntwork.io/a-crash-course-on-kubernetes-a96c3891ad82#355d>
- <https://blog.gruntwork.io/a-crash-course-on-kubernetes-a96c3891ad82#8418>
- <https://blog.gruntwork.io/a-crash-course-on-kubernetes-a96c3891ad82#c96d>
- <https://blog.gruntwork.io/a-crash-course-on-kubernetes-a96c3891ad82#0404>
- <https://blog.gruntwork.io/a-crash-course-on-kubernetes-a96c3891ad82#9ccb>
