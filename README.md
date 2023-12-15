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
## Local commands

## Set KUBECONFIG env var
export KUBECONFIG=~/Downloads/test-3-kubeconfig.yaml 

## Install the loadbalancer CONTROLLER
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.4/deploy/static/provider/cloud/deploy.yaml

## OR !!!
#helm upgrade --install ingress-nginx ingress-nginx \
#  --repo https://kubernetes.github.io/ingress-nginx \
#  --namespace ingress-nginx --create-namespace

### Wait 5 seconds and then run to get the IP
#
kubectl get service --namespace ingress-nginx ingress-nginx-controller --output wide --watch

## Cert Manager
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.3/cert-manager.yaml
## Check the pods
kubectl get pods --namespace cert-manager

# Confirm the IP
curl http://172.233.134.153
```

### DNS example

Usually would have DNS pointing to your IP, but here we can use nip.io
http://my-app.172-233-134-153.nip.io/

* edit `examples/static-nginx/ingress.yaml` with the nip domain name

```shell
# Now Apply (deploy) the sample app to the cluster
kustomize build examples/static-nginx/ | kubectl apply -f -
```

### Kustomize overlay example with multiple customers

DNS example, Get the IPs assigned to the ingress:
`kubectl get service --namespace ingress-nginx ingress-nginx-controller --output wide --watch`

1. edit the `examples/customer-examples/customer-overlays/customer-a/ingress.yaml` file with your nip domain name
2. edit the `examples/customer-examples/customer-overlays/customer-b/ingress.yaml` file with your nip domain name

```shell

# Now apply the two customers to the cluster
kustomize build examples/customer-examples/customer-overlays/customer-a | kubectl apply -f -
kustomize build examples/customer-examples/customer-overlays/customer-b | kubectl apply -f -

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


## Interesting Links

* https://stackoverflow.com/questions/54934095/where-is-the-full-kubernetes-yaml-spec
* https://www.linode.com/docs/guides/deploy-nginx-ingress-on-lke/
* https://www.linode.com/docs/products/compute/kubernetes/guides/load-balancing/
* https://www.linode.com/docs/guides/how-to-configure-load-balancing-with-tls-encryption-on-a-kubernetes-cluster/
* https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/
* https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/linode.md
* https://stackoverflow.com/questions/59844622/ingress-configuration-for-k8s-in-different-namespaces
* https://stackoverflow.com/questions/51878195/kubernetes-cross-namespace-ingress-network/51899301#51899301