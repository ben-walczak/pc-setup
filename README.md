# PC Setup

Purpose of this repository is to track how I have setup the infrastructure of my personal computers and to have a solid understanding and notes for bringing these things and ideas to an enterprise setting.

I always loved to teach, but I haven't gotten too much of an opportunity to do that with software quite yet. Teaching something at multiple levels of abstraction provides a newfound passion and understanding. And while I don't have the opportunity to teach at this moment, maybe I can teach myself. So here we go!

## Structure

This repository follows a hierarchical structure starting with there's no other requirements to start this application up to okay there's a lot of dependencies here. For the purpose of learning, I'm going to overengineer my personal setup where necessary to understand how these components and applications would work in an enterprise setting.

First-level applications (no dependency):
- [docker](https://github.com/ben-walczak/pc-setup/tree/main/docker) (most fundamental tool for building/creating images and running containers)
- [git](https://github.com/ben-walczak/pc-setup/tree/main/git) (minor public/private key setup)
- [minikube](https://github.com/ben-walczak/pc-setup/tree/main/minikube) (manage all containers the right way)
- [openssh](https://github.com/ben-walczak/pc-setup/tree/main/openssh) (linux heavy)
- [openvpn](https://github.com/ben-walczak/pc-setup/tree/main/openvpn) (linux heavy)

Second-level applications (one/two dependencies):
- [github-runner](https://github.com/ben-walczak/pc-setup/tree/main/github-runner) (docker/kubernetes)
- [gitlab-runner](https://github.com/ben-walczak/pc-setup/tree/main/gitlab-runner) (docker/kubernetes)
- [mongodb](https://github.com/ben-walczak/pc-setup/tree/main/mongodb) (docker/kubernetes)
- [registry](https://github.com/ben-walczak/pc-setup/tree/main/registry) (docker/kubernetes)
- [vault](https://github.com/ben-walczak/pc-setup/tree/main/vault) (docker/kubernetes)
- [pihole](https://github.com/ben-walczak/pc-setup/tree/main/pihole) (openvpn?)

Other:
- [bashrc](https://github.com/ben-walczak/pc-setup/tree/main/bashrc) (custom bash)

I have another repo for setting up a machine learning infrastructure. Most of the tools listed there live within minikube/kubernetes. Check it out [here](https://github.com/ben-walczak/ml-setup).

### Why is everything run in docker/kubernetes?
Well, if you want to transition your organization to a truly microservice oriented architecture, the best way to do so is to containerize everything. For more information on the benefits of containerization, namely kubernetes' container orchestration, check out my [minikube notes](https://github.com/ben-walczak/pc-setup/tree/main/minikube#minikube).

[This stackoverflow post](https://stackoverflow.blog/2020/05/29/why-kubernetes-getting-so-popular/) also does a good job of illustrating how there's never been one production technology infrastructure could agree on. This very well might be a rare case of software tool being here to stay.
