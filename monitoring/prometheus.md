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

Create a namespace dedicated to the monitoring stack.

Deploy Prometheus operator in it.

{% tabs %}
{% tab title="Command" %}
```bash
helm install coreos/prometheus-operator --name prometheus-operator --namespace monitoring 
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
NAME:   prometheus-operator
LAST DEPLOYED: Thu Apr 11 09:58:39 2019
NAMESPACE: monitoring
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                 DATA  AGE
prometheus-operator  1     2m22s

==> v1/Pod(related)
NAME                                  READY  STATUS   RESTARTS  AGE
prometheus-operator-545b59ffc9-d4gml  1/1    Running  0         2m22s

==> v1/ServiceAccount
NAME                 SECRETS  AGE
prometheus-operator  1        2m22s

==> v1beta1/ClusterRole
NAME                     AGE
prometheus-operator      2m22s
psp-prometheus-operator  2m22s

==> v1beta1/ClusterRoleBinding
NAME                     AGE
prometheus-operator      2m22s
psp-prometheus-operator  2m22s

==> v1beta1/Deployment
NAME                 READY  UP-TO-DATE  AVAILABLE  AGE
prometheus-operator  1/1    1           1          2m22s

==> v1beta1/PodSecurityPolicy
NAME                 PRIV   CAPS      SELINUX   RUNASUSER  FSGROUP    SUPGROUP  READONLYROOTFS  VOLUMES
prometheus-operator  false  RunAsAny  RunAsAny  MustRunAs  MustRunAs  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim


NOTES:
The Prometheus Operator has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "app=prometheus-operator,release=prometheus-operator"

Visit https://github.com/coreos/prometheus-operator for instructions on how
to create & configure Alertmanager and Prometheus instances using the Operator.
```
{% endtab %}
{% endtabs %}

Deploy the Prometheus stack in the dedicated namespace.

{% tabs %}
{% tab title="Command" %}
```bash
helm install coreos/kube-prometheus --name kube-prometheus --set global.rbacEnable=true --namespace monitoring
```
{% endtab %}

{% tab title="CLI Return" %}
```bash
NAME:   kube-prometheus
LAST DEPLOYED: Thu Apr 11 10:02:02 2019
NAMESPACE: monitoring
STATUS: DEPLOYED

RESOURCES:
==> v1/Alertmanager
NAME             AGE
kube-prometheus  1s

==> v1/ConfigMap
NAME                     DATA  AGE
kube-prometheus-grafana  10    2s

==> v1/Pod(related)
NAME                                                  READY  STATUS             RESTARTS  AGE
kube-prometheus-exporter-kube-state-5858d86974-5ps8z  0/2    ContainerCreating  0         1s
kube-prometheus-exporter-node-7c49t                   0/1    ContainerCreating  0         1s
kube-prometheus-grafana-6c4dffd84d-pjb6p              0/2    ContainerCreating  0         1s

==> v1/Prometheus
NAME             AGE
kube-prometheus  1s

==> v1/PrometheusRule
NAME                                              AGE
kube-prometheus                                   1s
kube-prometheus-alertmanager                      1s
kube-prometheus-exporter-kube-controller-manager  1s
kube-prometheus-exporter-kube-etcd                1s
kube-prometheus-exporter-kube-scheduler           1s
kube-prometheus-exporter-kube-state               1s
kube-prometheus-exporter-kubelets                 1s
kube-prometheus-exporter-kubernetes               1s
kube-prometheus-exporter-node                     1s
kube-prometheus-rules                             1s

==> v1/Secret
NAME                          TYPE    DATA  AGE
alertmanager-kube-prometheus  Opaque  1     2s
kube-prometheus-grafana       Opaque  2     2s

==> v1/Service
NAME                                              TYPE       CLUSTER-IP      EXTERNAL-IP  PORT(S)              AGE
kube-prometheus                                   ClusterIP  10.108.177.181  <none>       9090/TCP             2s
kube-prometheus-alertmanager                      ClusterIP  10.98.18.115    <none>       9093/TCP             2s
kube-prometheus-exporter-kube-controller-manager  ClusterIP  None            <none>       10252/TCP            2s
kube-prometheus-exporter-kube-dns                 ClusterIP  None            <none>       10054/TCP,10055/TCP  2s
kube-prometheus-exporter-kube-etcd                ClusterIP  None            <none>       4001/TCP             2s
kube-prometheus-exporter-kube-scheduler           ClusterIP  None            <none>       10251/TCP            2s
kube-prometheus-exporter-kube-state               ClusterIP  10.98.248.122   <none>       80/TCP               2s
kube-prometheus-exporter-node                     ClusterIP  10.103.171.99   <none>       9100/TCP             2s
kube-prometheus-grafana                           ClusterIP  10.98.4.110     <none>       80/TCP               2s

==> v1/ServiceAccount
NAME                                 SECRETS  AGE
kube-prometheus                      1        2s
kube-prometheus-exporter-kube-state  1        2s
kube-prometheus-exporter-node        1        2s
kube-prometheus-grafana              1        2s

==> v1/ServiceMonitor
NAME                                              AGE
kube-prometheus                                   0s
kube-prometheus-alertmanager                      1s
kube-prometheus-exporter-kube-controller-manager  1s
kube-prometheus-exporter-kube-dns                 1s
kube-prometheus-exporter-kube-etcd                1s
kube-prometheus-exporter-kube-scheduler           1s
kube-prometheus-exporter-kube-state               1s
kube-prometheus-exporter-kubelets                 1s
kube-prometheus-exporter-kubernetes               1s
kube-prometheus-exporter-node                     1s
kube-prometheus-grafana                           0s

==> v1beta1/ClusterRole
NAME                                     AGE
kube-prometheus                          2s
kube-prometheus-exporter-kube-state      2s
psp-kube-prometheus                      2s
psp-kube-prometheus-alertmanager         2s
psp-kube-prometheus-exporter-kube-state  2s
psp-kube-prometheus-exporter-node        2s
psp-kube-prometheus-grafana              2s

==> v1beta1/ClusterRoleBinding
NAME                                     AGE
kube-prometheus                          2s
kube-prometheus-exporter-kube-state      2s
psp-kube-prometheus                      2s
psp-kube-prometheus-alertmanager         2s
psp-kube-prometheus-exporter-kube-state  2s
psp-kube-prometheus-exporter-node        2s
psp-kube-prometheus-grafana              2s

==> v1beta1/DaemonSet
NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
kube-prometheus-exporter-node  1        1        0      1           0          <none>         2s

==> v1beta1/Deployment
NAME                                 READY  UP-TO-DATE  AVAILABLE  AGE
kube-prometheus-exporter-kube-state  0/1    1           0          2s
kube-prometheus-grafana              0/1    1           0          1s

==> v1beta1/PodSecurityPolicy
NAME                                 PRIV   CAPS      SELINUX   RUNASUSER  FSGROUP    SUPGROUP  READONLYROOTFS  VOLUMES
kube-prometheus                      false  RunAsAny  RunAsAny  MustRunAs  MustRunAs  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
kube-prometheus-alertmanager         false  RunAsAny  RunAsAny  MustRunAs  MustRunAs  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
kube-prometheus-exporter-kube-state  false  RunAsAny  RunAsAny  MustRunAs  MustRunAs  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
kube-prometheus-exporter-node        false  RunAsAny  RunAsAny  MustRunAs  MustRunAs  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim,hostPath
kube-prometheus-grafana              false  RunAsAny  RunAsAny  MustRunAs  MustRunAs  false     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim,hostPath

==> v1beta1/Role
NAME                                 AGE
kube-prometheus-exporter-kube-state  2s

==> v1beta1/RoleBinding
NAME                                 AGE
kube-prometheus-exporter-kube-state  2s


NOTES:
DEPRECATION NOTICE:

- alertmanager.ingress.fqdn is not used anymore, use alertmanager.ingress.hosts []
- prometheus.ingress.fqdn is not used anymore, use prometheus.ingress.hosts []
- grafana.ingress.fqdn is not used anymore, use prometheus.grafana.hosts []

- additionalRulesConfigMapLabels is not used anymore, use additionalRulesLabels
- prometheus.additionalRulesConfigMapLabels is not used anymore, use additionalRulesLabels
- alertmanager.additionalRulesConfigMapLabels is not used anymore, use additionalRulesLabels
- exporter-kube-controller-manager.additionalRulesConfigMapLabels is not used anymore, use additionalRulesLabels
- exporter-kube-etcd.additionalRulesConfigMapLabels is not used anymore, use additionalRulesLabels
- exporter-kube-scheduler.additionalRulesConfigMapLabels is not used anymore, use additionalRulesLabels
- exporter-kubelets.additionalRulesConfigMapLabels is not used anymore, use additionalRulesLabels
- exporter-kubernetes.additionalRulesConfigMapLabels is not used anymore, use additionalRulesLabels

```
{% endtab %}
{% endtabs %}

## Module exercise

## External documentation

Those documentations can help you to go further in this topic :

* Prometheus official documentation to [get started](https://prometheus.io/docs/prometheus/latest/getting_started/)

