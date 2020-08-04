# Nexus 3

[Sonatype Nexus 3](https://www.sonatype.com/nexus-repository-oss) is The free artefact repository with universal format support.

## Installing the Chart

Before you can install the chart you will need to add the `stevehipwell` repo to [Helm](https://helm.sh/).

```shell
helm repo add stevehipwell https://stevehipwell.github.io/helm-charts/
```

After you've installed the repo you can install the chart.

```shell
helm upgrade --install --namespace default --values ./my-values.yaml my-release stevehipwell/nexus3
```

## Configuration

The following table lists the configurable parameters of the _Nexus 3_ chart and their default values.

| Parameter                                 | Description                                                                                                 | Default           |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------- | ----------------- |
| `image.repository`                        | Docker repository to use                                                                                    | `sonatype/nexus3` |
| `image.tag`                               | Docker tag to use                                                                                           | `3.25.0`          |
| `image.pullPolicy`                        | Docker image pull policy                                                                                    | `IfNotPresent`    |
| `nameOverride`                            | String to partially override `nexus3.fullname` template (will prepend the release name)                     | `nil`             |
| `fullnameOverride`                        | String to fully override `nexus3.fullname` template                                                         | `nil`             |
| `securityContext.fsGroup`                 | File system group ownership                                                                                 | `200`             |
| `service.type`                            | Type of service                                                                                             | `ClusterIP`       |
| `service.port`                            | Service port                                                                                                | `8881`            |
| `service.additionalPorts`                 | Additional ports exposed by the service and used by repository connectors                                   | `nil`             |
| `caCerts.secret`                          | Name of the secret containing additional CA certificates                                                    | `nil`             |
| `metrics.enabled`                         | Metrics enabled for anonymous access                                                                        | `false`           |
| `metrics.serviceMonitor.enabled`          | Prometheus service monitor created                                                                          | `false`           |
| `metrics.serviceMonitor.additionalLabels` | Additional labels to be set on the ServiceMonitor                                                           | `{}`              |
| `envVars.jvmMaxRAMPercentage`             | JVM max RAM percentage                                                                                      | `25.0`            |
| `envVars.jvmMaxDirectMemorySize`          | JVM direct memory size                                                                                      | `2G`              |
| `env`                                     | List of environmental variable to apply to the deployment                                                   | `nil`             |
| `persistence.enabled`                     | Create a volume (PVC) for storage                                                                           | `false`           |
| `persistence.existingClaim`               | An existing PVC to use instead of creating a new one                                                        | `nil`             |
| `persistence.accessMode`                  | The PVC access mode                                                                                         | `ReadWriteOnce`   |
| `persistence.storageClass`                | The PVC storage class (use `-` for default)                                                                 | `standard`        |
| `persistence.size`                        | The size of the PVC to create                                                                               | `8Gi`             |
| `podAnnotations`                          | Pod Annotations                                                                                             | `{}`              |
| `resources`                               | Resource requests and limits                                                                                | `{}`              |
| `nodeSelector`                            | Node labels for pod assignment                                                                              | `{}`              |
| `tolerations`                             | List of node taints to tolerate                                                                             | `[]`              |
| `affinity`                                | Map of node/pod affinities                                                                                  | `{}`              |
| `ingress.enabled`                         | Create an ingress                                                                                           | `false`           |
| `ingress.annotations`                     | Annotations to enhance ingress configuration                                                                | `{}`              |
| `ingress.path`                            | Path for ingress rules                                                                                      | `/`               |
| `ingress.hosts`                           | List of ingress hosts                                                                                       | `[]`              |
| `ingress.tls`                             | List of TLS configurations (`ingress.tls[n].secretName`, `ingress.tls[n].hosts[m])`                         | `[]`              |
| `properties`                              | Additional _Nexus3_ properties.                                                                             | `nil`             |
| `config.enabled`                          | Automatically configure _Nexus3_.                                                                           | `false`           |
| `config.rootPassword.secret`              | The secret to use to update the root password (must also have `config.rootPassword.key` set).               | `nil`             |
| `config.rootPassword.key`                 | The key on the secret to use to update the root password (must also have `config.rootPassword.secret` set). | `nil`             |
| `config.anonymous.enabled`                | If _Nexus3_ should allow anonymous access.                                                                  | `false`           |
| `config.realms.enabled`                   | If the _Nexus3_ realms should be configured.                                                                | `false`           |
| `config.realms.values`                    | The _Nexus3_ realm ids to enable, in priority order.                                                        | `[]`              |
| `config.ldap.enabled`                     | If the _Nexus3_ LDAP should be configured.                                                                  | `false`           |
| `config.cleanup`                          | _Nexus3_ cleanup policies to be configured.                                                                 | `[]`              |
| `config.repos`                            | _Nexus3_ repos to be configured.                                                                            | `[]`              |
| `config.tasks`                            | _Nexus3_ tasks to be configured.                                                                            | `[]`              |

## Persistence

The [sonatype/nexus3](https://hub.docker.com/r/sonatype/nexus3/) image stores the _Nexus3_ data and configurations at the `/nexus-data` path in the container.

Persistent Volume Claims are used to keep the data across deployments. This is known to work in GCE, AWS, and minikube.
See the [Configuration](#configuration) section to configure the PVC or to disable persistence.