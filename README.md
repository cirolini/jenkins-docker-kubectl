# Jenkins docker container with docker and kubectl

This is a sample to use Jenkins in a Kubernetes CI/CD workflow to control your pipelines. Here you have a Docker container for Jenkins that have installed docker to build docker containers and kubectl to control your Kubernetes enviroment.

With this Jenkins you can build docker containers for your applications and put them in your Kubernetes enviroment.

## Getting Started

To develop this project you need Docker and a minimum kubernetes enviroment like minikube.

### To Build

To get a local version you need follow the steps:

```
$ git github.com/cirolini/jenkins-docker-kubectl
$ docker build -t jenkins-cicd .
```

### To Run Local

```
$ docker run -p 8080:8080 jenkins-cicd
```

or to mount a volume:

```
$ docker run -v ./jenkins_home:/var/jenkins_home -p 8080:8080 jenkins-cicd
```

#### How install

Open a web browser in http://localhost:8080/

```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                               NAMES
5ceb833c07cc        jenkins-cicd        "/usr/local/bin/jenk…"   About a minute ago   Up About a minute   0.0.0.0:8080->8080/tcp, 50000/tcp   vigorous_hofstadter

$ docker exec 5ceb833c07cc cat /var/jenkins_home/secrets/initialAdminPassword
025136ad4f5d42cda1102ecf93a73c7d
```

Install the suggested plugins e restart.

### How Run in a Kubernetes cluster

```
$ kubectl apply -f https://raw.githubusercontent.com/cirolini/jenkins-docker-kubectl/master/k8s_jenkins.yaml
```

This will create a Persistent Volume, a Service and Deployment to run Jenkins. And will apply the rbac role to grant Jenkins permissions for cluster admin.

I recommend that you install a registry to, for this follow this command:

```
$ kubectl apply -f https://raw.githubusercontent.com/cirolini/jenkins-docker-kubectl/master/k8s_registry.yaml
```

Now you have a Registry, this is a repository for storing and distributing Docker images.


#### Check instalation
```
$ kubectl get pods
NAME                       READY     STATUS    RESTARTS   AGE
jenkins-5ccb4f9498-56svc   1/1       Running   0          5m
registry-95c457bdb-9frmt   2/2       Running   0          5m
```

```
kubectl exec jenkins-5ccb4f9498-56svc cat /var/jenkins_home/secrets/initialAdminPassword
33c7f2604a274647acb327b87dba6427
```

### Example

This is a example of a project tha you can use in this Jenkins:
https://github.com/cirolini/Docker-Flask-uWSGI

And here have a tutorial to implement this case:
https://medium.com/@cirolini/entrega-continua-com-kubernetes-e-jenkins-84bd9834a749
