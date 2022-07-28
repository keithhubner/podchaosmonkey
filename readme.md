# Testing podchaosmonkey 🐵

## Introduction 👋

The purpose of this project is to spin up a Kubernetes cluster and test [podchaosmonkey](https://github.com/perithompson/podchaosmonkey).

### Tools / Setup 🛠

* [Civo CLI](https://www.civo.com/docs/cli)
* [kubectl](https://kubernetes.io/docs/tasks/tools/)
* [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [kubectx](https://github.com/ahmetb/kubectx)
* [docker](https://docs.docker.com/engine/install/)
* [go](https://go.dev/doc/install)
* [image repo e.g. dockerhub](https://hub.docker.com/)

* gcc

## Deploying the cluster 🚗

I will be deploying a managed Kubernetes cluster on Civo via the Civo CLI, for more information on how to setup an account and get started on Civo please see [this link](https://www.civo.com/docs/quick-start). 

Cluster node sizes can be changed and updated, so let's look at what is available.
```
civo kubernetes size
```

Then we can choose a suitable node size:

```
civo kubernetes create chaos_test -s g4s.kube.small --save --merge
```

Check the new cluster is our default:

```
kubectx 
```

Check we have access:
```
kubectl get pods -A
```

## Deployment 🚦

Next we can clone the repo

```
git clone https://github.com/perithompson/podchaosmonkey.git

```
CD into the directory:
```
cd podchaosmonkey
```

Next we can pretty much follow the [documentation in the repo](https://github.com/perithompson/podchaosmonkey):

Create a docker image and push it to a repo of your choice, i.e. dockerhub
```
make docker-build docker-push IMG=<some-registry>/podchaosmonkey:tag
```

Deploy the controller to the cluster with the image specified

```
make deploy IMG=<some-registry>/podchaosmonkey:tag
```

Deploy a sample workload

```
kubectl apply -f config/samples/sample-deployment.yaml
```

Create the Monkey resource

```
kubectl apply -f config/samples/podchaos_v1alpha1_monkey.yaml
```

Watch the Monkey do his thing! 🙈

```
watch kubectl get pods -n workloads
```

Have fun creating chaos! 🐵