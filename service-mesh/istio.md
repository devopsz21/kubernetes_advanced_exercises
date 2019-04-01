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

## External documentation

Those documentations can help you to go further in this topic :

* Istio official documentation to [get started](https://istio.io/docs/)

