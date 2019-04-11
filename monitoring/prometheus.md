# Prometheus

## Module

Prometheus is an open-source systems monitoring and alerting toolkit originally built at SoundCloud. It is now a standalone open source project and maintained independently of any company.

#### Overview

At the end of this module, you will :

* _Learn how to deploy Prometheus_
* _Learn how to monitor Kubernetes resources_
* _Learn how to manage metrics, rules and alerts_

#### Prerequisites

Create the directory `data/monitoring` in your home folder to manage the YAML file needed in this module.

```bash
mkdir ~/data/monitoring
```

## Deploy

### Manually

### Helm

#### Configure the Tiller

```bash
# Create a dedicated service account named tiller
kubectl -n kube-system create sa tiller

# Attach the role cluster-admin to the dedicated service account
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

# Upgrade the tiller configuration with the dedicated service account
helm init --service-account tiller --upgrade
```

#### Deploy Prometheus stack

```bash
# Create a namespace named monitoring and deploy the prometheus operator in it
helm install coreos/prometheus-operator --name prometheus-operator --namespace monitoring 

# Deploy prometheus stack in the dedicated namespace
helm install coreos/kube-prometheus --name kube-prometheus --set global.rbacEnable=true --namespace monitoring
```

## Module exercise

## External documentation

Those documentations can help you to go further in this topic :

* Prometheus official documentation to [get started](https://prometheus.io/docs/prometheus/latest/getting_started/)

