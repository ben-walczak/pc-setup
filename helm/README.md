# Helm

"Helm is an open source project created by DeisLabs and donated to the Cloud Native Computing Foundation, which now maintains it. Helm is a package manager designed specifically for Kubernetes and *improves the management of the YAML manifests* required to create Kubernetes projects." [Deepak Singh Dhami](https://www.techtarget.com/searchitoperations/tip/When-to-use-Kubernetes-operators-vs-Helm-charts)

Its important to note the **duality of helm**. On one hand, its like any package manager, but depending on your use case, you may also use it as a way to manage YAML manifests. This management mostly takes the form of template insertion, where you would automatically insert variables to a predefined YAML manifest.

**Operators**

While operators are a purely kubernetes construct, it fits really well with helm. It's easy to think of helm and operators as a this or that thing, but they really go hand in hand. An operator's purpose is to apply a custom controller to kubernetes manifests to automate workloads and control the behavior of these containers. Yeah... that's pretty confusing. Let's take a look at what [kubernetes docs](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/) are saying:

"The Operator pattern aims to capture the key aim of a human operator who is managing a service or set of services. Human operators who look after specific applications and services have deep knowledge of how the system ought to behave, how to deploy it, and how to react if there are problems.

People who run workloads on Kubernetes often like to use automation to take care of repeatable tasks. The Operator pattern captures how you can write code to automate a task beyond what Kubernetes itself provides."

Think of a stateless vs stateful application in kubernetes. With stateless applications, when pods die and restart everything acts as expected. There's no state that needs to be reloaded. The pod simply knows to create everything according to its configuration and nothing else. Whereas a stateful application needs to understand how to startup, restart, etc. according to some state. A common example of this is a database. There data that needs to maintained separate from kubernetes, where multiple pods can be interacting with it writing, reading, etc. This can come with a lot of complexity, and the best way to manage this is by leaving it to the experts. These experts can focus on creating a kubernetes operator via helm, which will help automate the management of a stateful application via custom kubernetes control loops among other things.

**Old vs. New**

Previously we would create all configuration YAML files yourself and execute them in the right order, which can be a lot of effort and require a step by step guide.

The new method is to simply `helm install` the operator and apply those files (with configuration added where needed).

## Getting started

Install via Homebrew (macOS)
```bash
brew install helm
```

Install via Chocolatey (Windows)
```bash
choco install kubernetes-helm
```

Install via Apt (Debian/Ubuntu)
```bash
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```
[Helm Docs](https://helm.sh/docs/intro/install/)

### How to install and apply a kubernetes operator
[Setup Prometheus Monitoring on Kubernetes using Helm and Prometheus Operator](https://www.youtube.com/watch?v=QoDqxm7ybLc):

```bash
helm install <custom-name> stable/prometheus-operator
kubectl get all
```


