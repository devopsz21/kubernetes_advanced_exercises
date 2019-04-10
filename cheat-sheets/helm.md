# Helm

## Install

There are two parts to Helm: The Helm client \(helm command line\) and the Helm server \(Tiller\).

### Client

The Helm client can be installed on all the principles operating systems : Linux, Mac and Windows.

{% tabs %}
{% tab title="Linux" %}
Helm has an installer script that will automatically grab the latest version of the Helm client and install it locally.

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```
{% endtab %}

{% tab title="Mac" %}
Members of the Kubernetes community have contributed a Helm formula build to Homebrew.

```bash
brew install kubernetes-helm
```
{% endtab %}

{% tab title="Windows" %}
Members of the Kubernetes community have contributed a Helm package build to Chocolatey.

```bash
choco install kubernetes-helm
```
{% endtab %}
{% endtabs %}

### Server

```bash
helm init
```

## External documentation

To go further in the management of Helm, please refer to these documentations :

* [Helm official documentation](https://helm.sh/)



