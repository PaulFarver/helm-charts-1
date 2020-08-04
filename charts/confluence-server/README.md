# Confluence Server

[Atlassian Confluence Server](https://www.atlassian.com/software/confluence) is your remote-friendly team workspace where knowledge and collaboration meet.

## Installing the Chart

Before you can install the chart you will need to add the `stevehipwell` repo to [Helm](https://helm.sh/).

```shell
helm repo add stevehipwell https://stevehipwell.github.io/helm-charts/
```

After you've installed the repo you can install the chart.

```shell
helm upgrade --install --namespace default --values ./my-values.yaml my-release stevehipwell/confluence-server
```

## Configuration

The following table lists the configurable parameters of the _Confluence Server_ chart and their default values.

| Parameter                    | Description                                                                                        | Default                                                 |
| ---------------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| `image.repository`           | Docker repository to use                                                                           | `atlassian/confluence-server`                           |
| `image.tag`                  | Docker tag to use                                                                                  | `7.6.2`                                                 |
| `image.pullPolicy`           | Docker image pull policy                                                                           | `IfNotPresent`                                          |
| `nameOverride`               | String to partially override `confluence-server.fullname` template (will prepend the release name) | `nil`                                                   |
| `fullnameOverride`           | String to fully override `confluence-server.fullname` template                                     | `nil`                                                   |
| `securityContext`            | The pod security context (all containers in the pod )                                              | `fsGroup: 2002`                                         |
| `service.type`               | Type of service                                                                                    | `ClusterIP`                                             |
| `service.port`               | Service port                                                                                       | `8080`                                                  |
| `caCerts.secret`             | Name of the secret containing additional CA certificates                                           | `nil`                                                   |
| `psql.host`                  | External _PostgreSQL_ host to use (if this is set the additional `psql` values need to be set)     | `nil`                                                   |
| `psql.port`                  | External _PostgreSQL_ port                                                                         | `nil`                                                   |
| `psql.database`              | External _PostgreSQL_ database                                                                     | `nil`                                                   |
| `psql.username`              | External _PostgreSQL_ username                                                                     | `nil`                                                   |
| `psql.password.secret`       | Name of the secret containing the external _PostgreSQL_ password                                   | `nil`                                                   |
| `psql.password.key`          | Key on the secret containing the external _PostgreSQL_ password                                    | `nil`                                                   |
| `envVars.jvmMinMemory`       | JVM minimum memory                                                                                 | `384M`                                                  |
| `envVars.jvmMaxMemory`       | JVM maximum memory                                                                                 | `768M`                                                  |
| `envVars.jvmMemoryOptions`   | JVM memory options                                                                                 | `-XX:MaxMetaspaceSize=512m -XX:MaxDirectMemorySize=10m` |
| `envVars.synchronyMaxMemory` | Synchrony maximum memory                                                                           | `0m`                                                    |
| `envVars.opts`               | Additional options, this can be used to set _CATALINA_OPTS_                                        | `""`                                                    |
| `env`                        | List of environmental variable to apply to the deployment                                          | `nil`                                                   |
| `persistence.enabled`        | Create a volume (PVC) for storage                                                                  | `false`                                                 |
| `persistence.existingClaim`  | An existing PVC to use instead of creating a new one                                               | `nil`                                                   |
| `persistence.accessMode`     | The PVC access mode                                                                                | `ReadWriteOnce`                                         |
| `persistence.storageClass`   | The PVC storage class (use `-` for default)                                                        | `standard`                                              |
| `persistence.size`           | The size of the PVC to create                                                                      | `8Gi`                                                   |
| `podAnnotations`             | Pod Annotations                                                                                    | `{}`                                                    |
| `resources`                  | Resource requests and limits                                                                       | `{}`                                                    |
| `nodeSelector`               | Node labels for pod assignment                                                                     | `{}`                                                    |
| `tolerations`                | List of node taints to tolerate                                                                    | `[]`                                                    |
| `affinity`                   | Map of node/pod affinities                                                                         | `{}`                                                    |
| `ingress.enabled`            | Create an ingress                                                                                  | `false`                                                 |
| `ingress.annotations`        | Annotations to enhance ingress configuration                                                       | `{}`                                                    |
| `ingress.path`               | Path for ingress rules                                                                             | `/`                                                     |
| `ingress.hosts`              | List of ingress hosts                                                                              | `[]`                                                    |
| `ingress.tls`                | List of TLS configurations (`ingress.tls[n].secretName`, `ingress.tls[n].hosts[m])`                | `[]`                                                    |
| `postgresql.enabled`         | Enable _PostgreSQL_ sub chart                                                                      | `false`                                                 |

## Database

By default the [atlassian/confluence-server](https://hub.docker.com/r/atlassian/confluence-server/) image will create a [H2](https://www.h2database.com/html/main.html) database for evaluation purposes, this should **NOT be used in production**. You can either allow this chart to create a [PostgreSQL](https://hub.docker.com/_/postgres) instance specifically for _Confluence Server_ by specifying `postgresql.enabled` as `true` or you can use an external _PostgreSQL_ instance by specifying the connection details on `psql`.

## Persistence

The [atlassian/confluence-server](https://hub.docker.com/r/atlassian/confluence-server/) image stores the _Confluence Server_ data and configurations at the `/var/atlassian/application-data/confluence` path in the container.

Persistent Volume Claims are used to keep the data across deployments. This is known to work in GCE, AWS, and minikube.
See the [Configuration](#configuration) section to configure the PVC or to disable persistence.