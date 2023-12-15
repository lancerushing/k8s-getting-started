# Getting Started with Kubernetes

## TL;DR

Create a cluster, then use `kubectl` to manage it. see: https://www.linode.com/docs/products/compute/kubernetes/get-started/

1. Local Setup

* Install kubectl (mandatory)
* Install kustomize
* Install kubectx (useful)

2. Remote Setup

* Create a cluster in linode
* Download the "Kubeconfig" for the cluster

3. Put it all together

`kustomize `



## Local Workstation Instructions

### Install kubectl

https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

### Install kubectx

### Install Kustomize

https://kubectl.docs.kubernetes.io/installation/kustomize/

Tool for maniuplating k8s yaml in a predictable way.  


## Kubernetes Resources (Jargon)


### Namespace

https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

A namespace for resourcee. most resources are namespaced, but management resources are not.

### POD

https://kubernetes.io/docs/concepts/workloads/pods/

Where the programs actually run in containers. Pods have 1 or more containers. Intra POD containers use localhost to talk to each other.  Example: nginx web server container with php-fpm container.

We usually don't create PODs directly with yaml, but use POD Templates inside of deployments or batch jobs.

### Deployment

https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

An application definition used to scale and set required variables and stuff.

USE THIS to roll out your web apps!!

### Service

https://kubernetes.io/docs/concepts/services-networking/

Networking "glue" layer.  PODS that handle incoming network connections will usually expose their ports with a named service. POD to POD conections. Exteral web services, etc, etc.

## Kustomize

https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/

## Additional things to learn in the Future

* Wire up Continuous Delivery
  * ArgoCD
  * FluxCD




## Insteresting Links

* https://stackoverflow.com/questions/54934095/where-is-the-full-kubernetes-yaml-spec
