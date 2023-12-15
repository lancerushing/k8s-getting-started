# Getting Started with Kubernetes

## TL;DR

Create a cluster, then use `kubectl` to manage it. see: https://www.linode.com/docs/products/compute/kubernetes/get-started/

1. Local Setup

* Install kubectl
* Install kustomize
* Install helm
* Install kubectx (useful)

2. Remote Setup

* Create a cluster in linode
* Download the "Kubeconfig" for the cluster

3. Put it all together


```shell

## Install the loadbalancer CONTROLLER
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx

## Cert Manager
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.3/cert-manager.yaml
kubectl get pods --namespace cert-manager



## Wait 30 seconds and then run
kubectl get service --namespace default ingress-nginx-controller --output wide --watch

#Check the IP:
curl http://172.233.134.149

# Now Apply (deploy) our app to the cluster
kustomize build examples/static-nginx/ | kubectl apply -f -


http://172.233.134.149.nip.io/

http://172-233-134-149.nip.io/


```

## Local Workstation Instructions

### Install kubectl

https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

```shell
sudo port selfupdate
sudo port install kubectl
```

### Install helm

Useful for install third party apps into the cluster.

https://helm.sh/docs/intro/install/

### Install Kustomize

https://kubectl.docs.kubernetes.io/installation/kustomize/

Tool for maniuplating k8s yaml in a predictable way.  

```shell
sudo port install kustomize
```

### Install kubectx


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

### ingress

Creates an incoming network connection handler.  Ingresses are provider specific on external implentation.  Linode will create a load balancer, AWS will create an ELB, etc.

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx


## Kustomize

https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/

## Additional things to learn in the Future

* Wire up Continuous Delivery
  * ArgoCD
  * FluxCD


## Insteresting Links

* https://stackoverflow.com/questions/54934095/where-is-the-full-kubernetes-yaml-spec
* https://www.linode.com/docs/guides/deploy-nginx-ingress-on-lke/
* https://www.linode.com/docs/products/compute/kubernetes/guides/load-balancing/
* https://www.linode.com/docs/guides/how-to-configure-load-balancing-with-tls-encryption-on-a-kubernetes-cluster/
