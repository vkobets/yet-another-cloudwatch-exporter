# Cloudwatch exporter

* Installs [YACE - yet another cloudwatch exporter](https://github.com/nerdswords/yet-another-cloudwatch-exporter)

## TL;DR;

```console
$ helm repo add nerdswords https://nerdswords.github.io/yet-another-cloudwatch-exporter
$ helm install nerdswords/yet-another-cloudwatch-exporter
```

## Introduction

This chart bootstraps a [YACE - yet another cloudwatch exporter](https://github.com/nerdswords/yet-another-cloudwatch-exporter) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- [kube2iam](https://github.com/jtblin/kube2iam) installed to use the **aws.role** config option otherwise configure **aws.aws_access_key_id** and **aws.aws_secret_access_key** or **aws.secret.name**
- [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) installed to use serviceMonitor and/or prometheusRule

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm repo add nerdswords https://nerdswords.github.io/yet-another-cloudwatch-exporter
$ # pass AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY as values
$ helm install --name my-release nerdswords/yet-another-cloudwatch-exporter --set aws.aws_access_key_id=$AWS_ACCESS_KEY_ID,aws.aws_secret_access_key=$AWS_SECRET_ACCESS_KEY

$ # or store them in a secret and pass its name as a value
$ kubectl create secret generic <SECRET_NAME> --from-literal=access_key=$AWS_ACCESS_KEY_ID --from-literal=secret_key=$AWS_SECRET_ACCESS_KEY
$ helm install --name my-release nerdswords/yet-another-cloudwatch-exporter --set aws.secret.name=<SECRET_NAME>

$ # or add a role to aws with the [correct policy](https://github.com/prometheus/cloudwatch_exporter#credentials-and-permissions) to add to cloud watch and pass its name as a value
$ helm install --name my-release nerdswords/yet-another-cloudwatch-exporter --set aws.role=<ROLL_NAME>
```

The command deploys YACE exporter on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the YACE Exporter chart and their default values.

| Parameter                         | Description                                                             | Default                     |
| --------------------------------- | ----------------------------------------------------------------------- | --------------------------- |
| `image.repository`                | Image                                                                   | `quay.io/invisionag/yet-another-cloudwatch-exporter`  |
| `image.tag`                       | Image tag                                                               | `v0.13.7`                   |
| `image.pullPolicy`                | Image pull policy                                                       | `IfNotPresent`              |
| `command`                         | Container entrypoint command                                            | `[]`                        |
| `service.type`                    | Service type                                                            | `ClusterIP`                 |
| `service.port`                    | The service port                                                        | `80`                        |
| `resources`                       |                                                                         | `{}`                        |
| `extraArgs`                       | Additional container arguments                                          | `[]`                        |
| `aws.role`                        | AWS IAM Role To Use                                                     |                             |
| `aws.aws_access_key_id`           | AWS access key id                                                       |                             |
| `aws.aws_secret_access_key`       | AWS secret access key                                                   |                             |
| `aws.secret.name`                 | The name of a pre-created secret in which AWS credentials are stored    |                             |
| `aws.secret.includesSessionToken` | Whether or not the pre-created secret contains an AWS STS session token |                             |
| `config`                          | YACE configuration (default found in the webpage)                       | `example configuration`     |
| `serviceAccount.create`           | Specifies whether a service account should be created.                  | `true`                      |
| `serviceAccount.name`             | Name of the service account.                                            |                             |
| `serviceAccount.annotations`      | Custom annotations for service                                          | `{}`                        |
| `serviceAccount.labels`           | Additional custom labels for the service                                | `{}`                        |
| `tolerations`                     | Add tolerations                                                         | `[]`                        |
| `nodeSelector`                    | node labels for pod assignment                                          | `{}`                        |
| `affinity`                        | node/pod affinities                                                     | `{}`                        |
| `livenessProbe`                   | Liveness probe settings                                                 |                             |
| `readinessProbe`                  | Readiness probe settings                                                |                             |
| `serviceMonitor.enabled`          | Use servicemonitor from prometheus operator                             | `false`                     |
| `serviceMonitor.namespace`        | Namespace thes Servicemonitor  is installed in                          |                             |
| `serviceMonitor.interval`         | How frequently Prometheus should scrape                                 |                             |
| `serviceMonitor.telemetryPath`    | path to yave telemtery-path                                             |                             |
| `serviceMonitor.labels`           | labels for the ServiceMonitor passed to Prometheus Operator             | `{}`                        |
| `serviceMonitor.timeout`          | Timeout after which the scrape is ended                                 |                             |
| `ingress.enabled`                 | Enables Ingress                                                         | `false`                     |
| `ingress.annotations`             | Ingress annotations                                                     | `{}`                        |
| `ingress.labels`                  | Custom labels                                                           | `{}`                        |
| `ingress.hosts`                   | Ingress accepted hostnames                                              | `[]`                        |
| `ingress.tls`                     | Ingress TLS configuration                                               | `[]`                        |
| `podAnnotations`                  | Custom pod annotations                                                  | `{}`                        |
| `podLabels`                       | Custom pod labels                                                       | `{}`                        |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install --name my-release \
    --set aws.role=my-aws-role \
    nerdswords/yet-another-cloudwatch-exporter
```

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml nerdswords/yet-another-cloudwatch-exporter
```
