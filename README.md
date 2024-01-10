# Cartographer

![Test Workflow](https://github.com/kadras-io/package-for-cartographer/actions/workflows/test.yml/badge.svg)
![Release Workflow](https://github.com/kadras-io/package-for-cartographer/actions/workflows/release.yml/badge.svg)
[![The SLSA Level 3 badge](https://slsa.dev/images/gh-badge-level3.svg)](https://slsa.dev/spec/v1.0/levels)
[![The Apache 2.0 license badge](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Follow us on Twitter](https://img.shields.io/static/v1?label=Twitter&message=Follow&color=1DA1F2)](https://twitter.com/kadrasIO)

A Carvel package for [Cartographer](https://cartographer.sh), a cloud-native framework to build paved paths to production on Kubernetes.

## 🚀&nbsp; Getting Started

### Prerequisites

* Kubernetes 1.27+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/carvel-dev/kapp-controller/releases/latest/download/release.yml
  ```

### Dependencies

Cartographer requires [cert-manager](https://github.com/kadras-io/package-for-cert-manager). You can install it from the [Kadras package repository](https://github.com/kadras-io/kadras-packages).

### Installation

Add the Kadras [package repository](https://github.com/kadras-io/kadras-packages) to your Kubernetes cluster:

  ```shell
  kctrl package repository add -r kadras-packages \
    --url ghcr.io/kadras-io/kadras-packages \
    -n kadras-packages --create-namespace
  ```

<details><summary>Installation without package repository</summary>
The recommended way of installing the Cartographer package is via the Kadras <a href="https://github.com/kadras-io/kadras-packages">package repository</a>. If you prefer not using the repository, you can add the package definition directly using <a href="https://carvel.dev/kapp/docs/latest/install"><code>kapp</code></a> or <code>kubectl</code>.

  ```shell
  kubectl create namespace kadras-packages
  kapp deploy -a cartographer-package -n kadras-packages -y \
    -f https://github.com/kadras-io/package-for-cartographer/releases/latest/download/metadata.yml \
    -f https://github.com/kadras-io/package-for-cartographer/releases/latest/download/package.yml
  ```
</details>

Install the Cartographer package:

  ```shell
  kctrl package install -i cartographer \
    -p cartographer.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-packages
  ```

> **Note**
> You can find the `${VERSION}` value by retrieving the list of package versions available in the Kadras package repository installed on your cluster.
> 
>   ```shell
>   kctrl package available list -p cartographer.packages.kadras.io -n kadras-packages
>   ```

Verify the installed packages and their status:

  ```shell
  kctrl package installed list -n kadras-packages
  ```

## 📙&nbsp; Documentation

Documentation, tutorials and examples for this package are available in the [docs](docs) folder.
For documentation specific to Cartographer, check out [cartographer.sh](http://cartographer.sh).

## 🎯&nbsp; Configuration

The Cartographer package can be customized via a `values.yml` file.

  ```yaml
  cartographer:
    concurrency:
      max_workloads: 10
      max_deliveries: 10
  ```

Reference the `values.yml` file from the `kctrl` command when installing or upgrading the package.

  ```shell
  kctrl package install -i cartographer \
    -p cartographer.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-packages \
    --values-file values.yml
  ```

### Values

The Cartographer package has the following configurable properties.

<details><summary>Configurable properties</summary>

| Config | Default | Description |
|-------|-------------------|-------------|
| `optional_components.cartographer_conventions` | `true` | Whether to deploy the Cartographer Conventions component. |
| `ca_cert_data` | `""` | PEM-encoded certificate data to trust TLS connections with a custom CA. |
| `logging.level` | `info` | Log verbosity level. Options: `debug`, `info`, `error`. |

Settings for the Cartographer component.

| Config | Default | Description |
|-------|-------------------|-------------|
| `cartographer.concurrency.max_workloads` | `2` | Maximum concurrent Workloads processed by the Cartographer controller. |
| `cartographer.concurrency.max_runnables` | `2` | Maximum concurrent Runnables processed by the Cartographer controller. |
| `cartographer.concurrency.max_deliveries` | `2` | Maximum concurrent Deliveries processed by the Cartographer controller. |
| `cartographer.resources.requests.cpu` | `500m` | CPU requests configuration for the Cartographer controller. |
| `cartographer.resources.requests.memory` | `512Mi` | Memory requests configuration for the Cartographer controller. |
| `cartographer.resources.limits.cpu` | `1` | CPU limits configuration for the Cartographer controller. |
| `cartographer.resources.limits.memory` | `1Gi` | Memory limits configuration for the Cartographer controller. |

Settings for the Cartographer Conventions component.

| Config | Default | Description |
|-------|-------------------|-------------|
| `conventions.resources.requests.cpu` | `100m` | CPU requests configuration for the Cartographer Conventions controller. |
| `conventions.resources.requests.memory` | `20Mi` | Memory requests configuration for the Cartographer Conventions controller. |
| `conventions.resources.limits.cpu` | `100m` | CPU limits configuration for the Cartographer Conventions controller. |
| `conventions.resources.limits.memory` | `256Mi` | Memory limits configuration for the Cartographer Conventions controller. |

</details>

## 🛡️&nbsp; Security

The security process for reporting vulnerabilities is described in [SECURITY.md](SECURITY.md).

## 🖊️&nbsp; License

This project is licensed under the **Apache License 2.0**. See [LICENSE](LICENSE) for more information.

## 🙏&nbsp; Acknowledgments

This package is based on the original Cartographer package used in the [Tanzu Community Edition](https://github.com/vmware-tanzu/community-edition) project before its retirement.
