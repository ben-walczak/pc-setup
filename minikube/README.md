# Minikube

**What?**
As the name suggests, minikube is a miniture version of kubernetes. Minikube runs locally on your personal machine, whereas kubernetes is built to work on a massive amount of clusters on the cloud. While you can create and manages namespaces in minikube, it only works on a single-node cluster on your local PC within a virtual machine.

**Why?**
"Kuberenetes is a portable, extensible, open source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation." - [kubernetes docs](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)

Kubernetes is the natural transition from traditional deployment -> to virtualized deployment -> to containerized deployment. Rather than running an application on an operating system or a virtual machine on a hypervisor (on the os), we can now run a lightweight container on a container runtime (on the os). Essentially, traditional deployment was messy because you had little control and variation of the operating system, and virtualized deployment was heavy and required a virtualized operating system installed.

Benefits of kubernetes:
- Availability *bc pods have failover mechanisms and methods of deployment*
- Scalability *bc you can simply increase replicaset*
- Debugability *bc ability to debug failed pods and look at logs*
- Consistentency *bc manifests can be described in yaml files and stored via code (configuration as code)*

Main benefits of minikube as to kubernetes:
- Minikube allows you to learn and experiment quickly.
- Minikube may be used to examine key kubernetes functionality.
- Experiment with Kubernetes' extensibility.
- Minikube can help you walk the talk.
- Minikube can be used as a proof-of-concept tool.

## Getting started

### Requirements
- 2 CPU's or more
- 2 GB of free memory
- 20 GB of free disk space
- Internet connection
- Container or virtual machine manager

### Installation

Check out minikube's [documentation](https://minikube.sigs.k8s.io/docs/start/) for specific binaries.

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### Start Cluster
```bash
minikube start
```

### Use version of kubectl
```bash
minikube kubectl -- <kubectl command>
alias kubectl="minikube kubectl --"
minikube dashboard
```

### Utilize minikube with local images
1. Set the environment variables with `eval $(minikube docker-env)
2. Build the image with the Docker daemon of Minikube (eg `docker build -t my-image .`)
3. Set the image in the pod spec like the build tag (eg `my-image`)
4. Set the `imagePullPolicy` to `Never`, otherwise Kubernetes will try to downloa the image
- [Source](https://stackoverflow.com/questions/42564058/how-to-use-local-docker-images-with-minikube)

### Pull Image from Private Docker Registry in Kubernetes cluster
1. Create `Secret` component that contains credentials for Docker registry

```bash
docker login -u <username> -p <password> <url of registry>
cat .docker/config.json # needed for secret, where credentials here are used in secret
```

Login to private repository within minikube
```bash
minikube ssh
pwd
<paste login command here>
ls -la # .docker directory should be created
cat .docker/config.json # verify with this
```

Create yaml file for secret
```yaml
apiVersion: v1
kind: Secret
metadata:
    name: my-registry-key
data:
    .dockerconfigjson: <paste secret here>
type: kubernetes.io/dockerconfigjson
```

Secure copy .docker/config.json to minikube
```bash
scp -i $(minikube ssh-key) docker@$(minikube ip):.docker/config.json .docker/config.json
cat .docker/config.json | base64
```
*Copy and paste the output to .dockerconfigjson*

```bash
kubectl create secret generic my-registry-key \
    --from-file=.dockerconfigjson=.docker/config.json \
    --type=kubernetes.io/dockerconfigjson
kubectl get secret
```

All in one step
```bash
kubectl create secret docker-registry my-registry-key-two \
    --docker-server=<url to docker registry>
    --docker-username=<username> \
    --docker-password=<password>
```

2. Configure Deployment/Pod to use `Secrets` using `imagePullSecrets`
- [Source](https://www.youtube.com/watch?v=asIS4KIs40M)

Yaml file for deployment, which references the image stored in secrets
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      imagePullSecrets:
      - name: my-registry-key
      containers:
      - name: my-app
        image: private-repo/my-app:1.3
        imagePullPolicy: Always
        ports:
          - containerPort: 3000

```