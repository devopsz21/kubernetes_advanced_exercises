# Istio

## Module

Istio is a service mesh created through a collaboration between IBM, Google and Lyft. It uses the sidecar pattern, where sidecars are enabled by the Envoy proxy and are based on containers. By injecting Envoy proxy servers into the network path between services, Istio provides sophisticated traffic management controls, such as load-balancing and fine-grained routing.

#### Overview

At the end of this module, you will :

* _Learn how to deploy Istio on Kubernetes_
* _Learn how to manage service accessibility_
* _Learn how to get observability on the Kubernetes cluster_

#### Prerequisites

Create the directory `data/servicemesh` in your home folder to manage the YAML file needed in this module.

```bash
mkdir ~/data/servicemesh
```

#### Minikube

To deploy Istio on Minikube, the default configuration of Minikube has to be upgraded to fit Istio need's.

```bash
minikube config set cpus 4
minikube config set memory 8192
minikube config set disk-size 50g
minikube addons enable ingress 
minikube start
```

## Client

### Installation

{% tabs %}
{% tab title="Command" %}
```bash
# Move to the module directory
cd ~/data/servicemesh/

# Download Istio packages
curl -L https://git.io/getLatestIstio | sh -

# Export istio version in environment variable
export "ISTIO_VERSION=$(ls . | grep istio | cut -d "-" -f2)"
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
Downloading istio-1.1.1 from https://github.com/istio/istio/releases/download/1.1.1/istio-1.1.1-linux.tar.gz ...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   614    0   614    0     0   2646      0 --:--:-- --:--:-- --:--:--  2646
100 15.0M  100 15.0M    0     0   9.7M      0  0:00:01  0:00:01 --:--:-- 13.8M
Downloaded into istio-1.1.1:
bin  install  istio.VERSION  LICENSE  README.md  samples  tools
Add /home/yourusername/data/servicemesh/istio-1.1.1/bin to your path; e.g copy paste in your shell and/or ~/.profile:
export PATH="$PATH:/home/yourusername/data/servicemesh/istio-1.1.1/bin"
```
{% endtab %}
{% endtabs %}

### Profile configuration

```bash
# Configure temporarily the current shell session
export PATH=$PWD/istio-$ISTIO_VERSION/bin:$PATH

# Configure permanently the shell session
echo "PATH=$PATH:$PWD/istio-$ISTIO_VERSION/bin:$PATH" >> ~/.profile
source ~/.profile
```

## Server

### Installation

```bash
kubectl create -f ~/data/servicemesh/istio-$ISTIO_VERSION/install/kubernetes/helm/istio/templates/crds.yaml
kubectl create -f ~/data/servicemesh/istio-$ISTIO_VERSION/install/kubernetes/istio-demo.yaml
```

### Get

```bash
kubectl get pod -n istio-system
```

## External documentation

Those documentations can help you to go further in this topic :

* Istio official documentation to [get started](https://istio.io/docs/)

