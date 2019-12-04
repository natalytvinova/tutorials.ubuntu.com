---
id: using-helm-with-canonical-kubernetes
summary: Learn how to deploy Helm on Charmed Kubernetes
categories: containers
tags: juju, kubernetes, beginner, container, helm, charts
difficulty: 3
status: draft
author: Natalia Litvinova <natalia.litvinova@canonical.com>
feedback_url: https://github.com/canonical-websites/tutorials.ubuntu.com/issues
published: 2019-12-03

---
# Using Helm with Canonical Kubernetes

## Overview
Duration: 2:00

Helm is a tool that streamlines installing and managing Kubernetes applications. Think of it like apt/yum/homebrew for Kubernetes. It allows describing the application structure through convenient helm-charts and managing it with simple commands.

### What you'll learn

- How to deploy Helm 
- How to deploy a chart

### You will only need

- a Charmed Kubernetes cluster

positive
: If you do not have a Charmed Kubernetes cluster, you can refer to the following [tutorial](https://tutorials.ubuntu.com/tutorial/get-started-charmed-kubernetes) to spin up one in minutes. Charmed Kubernetes is a production-grade Kubernetes offering from Canonical which is fully compliant with the upstream project. Get Charmed Kubernetes right away and benefit from simplified deployments and operations provided by Juju charms.

Survey
: How will you use this tutorial?
 - Only read through it
 - Read it and complete the exercises
: What is your current level of experience?
 - Novice
 - Intermediate
 - Proficient

## Prepare your cluster
Duration: 2:00

Make sure that your Kubernetes cluster is running and kubectl config is in ~/.kube/config.

`juju scp kubernetes-master/0:config ~/.kube/config`

Also make sure you have opened the Helm port 44134 on all your worker nodes:

```
juju run --unit kubernetes-worker/0 "open-port 44134"
juju run --unit kubernetes-worker/1 "open-port 44134"
juju run --unit kubernetes-worker/2 "open-port 44134"
```

## Deploy Helm
Duration: 5:00

We can deploy Helm using the snap or from an installer script.

### Deploy using snap 

```
sudo apt-get install snapd
sudo snap install helm
```

### Deploy from script

Helm now has an [installer script](https://github.com/helm/helm/blob/master/scripts/get-helm-3) that will automatically grab the latest version of Helm and install it locally. You can fetch that script and then execute it locally. It’s well documented so that you can read through it and understand what it is doing before you run it.

```
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```

This will download Helm from GitHub and deploy the binary. Helm should now be ready to roll on your cluster.

## Deploying a Chart
Duration: 7:00

A chart is a Helm package that contains information sufficient for installing a set of Kubernetes resources into a Kubernetes cluster. Charts contain a Chart.yaml file as well as templates, default values (values.yaml), and dependencies.
After release 3.0.0 Helm changed the experience with repositories and now no longer includes stable charts repository by default. 
So one of the first things to do is to add repositories that contain charts that we want. We can search [Helm Hub](https://hub.helm.sh/) and usually chart's page contains the information about the repository.
And we can also add stable and incubator repositories which will be supported by Helm Hub until May 13, 2020:

```
helm repo add stable https://kubernetes-charts.storage.googleapis.com
helm repo add incubator http://storage.googleapis.com/kubernetes-charts-incubator
```

After repositories are added we need to run an update to get the latest charts:

`helm repo update`

Next we can search charts in our repos:

```
helm search repo docker-registry
NAME                     	CHART VERSION	APP VERSION	DESCRIPTION                     
incubator/docker-registry	0.3.0        	2.6.2      	A Helm chart for Docker Registry
stable/docker-registry   	1.8.3        	2.7.1      	A Helm chart for Docker Registry
```

And finally we can install a simple chart:

`helm install docker-registry stable/docker-registry`

### Writing a Chart
If you want to write your own chart take a look at the full guide provided in the [Helm Manual](https://helm.sh/docs/topics/charts/).

## That's all folks!
Duration: 1:00

Congratulations! In this tutorial, you installed Helm on Charmed Kubernetes and deployed a simple docker-registry chart.

### Where to go from here?
- Explore [Charmed Kubernetes](https://jaas.ai/kubernetes)
- Use Kubernetes straight away for free with [MicroK8s](https://microk8s.io)
- Looking for production-grade [Kubernetes?](https://ubuntu.com/kubernetes/contact-us)
- Find out more about Helm in [documentation](https://docs.helm.sh)
