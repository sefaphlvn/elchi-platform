[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/elchi-platform)](https://artifacthub.io/packages/search?repo=elchi-platform)

# Elchi Platform Helm Charts

This repository contains Helm charts for deploying the Elchi Platform components.

## Global Values

| Parameter | Description | Default |
|-----------|-------------|---------|
| `global.namespace` | Namespace where all components will be deployed | `"elchi-platform"` |
| `global.mainURL` | Base URL for the all components | `"bigbang.elchi.io"` |
| `global.port` | Port for the BigBang API. If empty, uses 80/443 based on TLS | `""` |
| `global.tlsEnabled` | Whether to use HTTPS | `false` |
| `global.installMongo` | Whether to use self-hosted MongoDB | `true` |
| `global.versions` | List of BigBang versions to deploy | `[v0.1.0-v0.13.4-envoy1.33.0, v0.1.0-v0.13.4-envoy1.32.3]` |
| `global.mongodb.*` | MongoDB connection settings | See MongoDB section |
| `global.bigbang.grpcDefaultReplicas` | Default replicas for gRPC services | `3` |
| `global.bigbang.restDefaultReplicas` | Default replicas for REST services | `2` |

## BigBang Chart Values

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | BigBang image repository | `"spehlivan/bigbang"` |
| `image.pullPolicy` | Image pull policy | `"IfNotPresent"` |
| `resources.rest.requests.memory` | REST service memory request | `"256Mi"` |
| `resources.rest.requests.cpu` | REST service CPU request | `"150m"` |
| `resources.rest.limits.memory` | REST service memory limit | `"512Mi"` |
| `resources.rest.limits.cpu` | REST service CPU limit | `"250m"` |
| `resources.grpc.requests.memory` | gRPC service memory request | `"256Mi"` |
| `resources.grpc.requests.cpu` | gRPC service CPU request | `"150m"` |
| `resources.grpc.limits.memory` | gRPC service memory limit | `"512Mi"` |
| `resources.grpc.limits.cpu` | gRPC service CPU limit | `"250m"` |
| `service.type` | Kubernetes service type | `"ClusterIP"` |
| `service.rest.port` | REST service port | `8099` |
| `service.grpc.port` | gRPC service port | `18000` |
| `config.logging.level` | Logging level | `"info"` |
| `config.logging.formatter` | Log formatter type | `"text"` |
| `config.logging.reportCaller` | Whether to report caller in logs | `"false"` |

## Elchi Chart Values

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | Elchi image repository | `"spehlivan/elchi"` |
| `image.tag` | Elchi image tag | `"latest"` |
| `image.pullPolicy` | Image pull policy | `"IfNotPresent"` |
| `service.type` | Kubernetes service type | `"ClusterIP"` |
| `service.port` | Service port | `80` |
| `resources.replicas` | Number of Elchi replicas | `3` |
| `resources.requests.memory` | Memory request | `"128Mi"` |
| `resources.requests.cpu` | CPU request | `"100m"` |
| `resources.limits.memory` | Memory limit | `"256Mi"` |
| `resources.limits.cpu` | CPU limit | `"200m"` |

## Envoy Chart Values

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicas` | Number of Envoy replicas | `4` |
| `image.repository` | Envoy image repository | `"envoyproxy/envoy"` |
| `image.tag` | Envoy image tag | `"v1.33.0"` |
| `image.pullPolicy` | Image pull policy | `"IfNotPresent"` |
| `service.type` | Kubernetes service type | `"NodePort"` |
| `service.httpPort` | HTTP port | `30080` |
| `service.adminPort` | Admin port | `30081` |
| `resources.requests.memory` | Memory request | `"128Mi"` |
| `resources.requests.cpu` | CPU request | `"100m"` |
| `resources.limits.memory` | Memory limit | `"256Mi"` |
| `resources.limits.cpu` | CPU limit | `"200m"` |

## MongoDB Chart Values

| Parameter | Description | Default |
|-----------|-------------|---------|
| `auth.rootPassword` | MongoDB root password | `"elchi"` |
| `auth.username` | MongoDB username | `"elchi"` |
| `auth.password` | MongoDB password | `"elchi"` |
| `auth.database` | MongoDB database name | `"elchi"` |
| `persistence.enabled` | Enable persistence | `true` |
| `persistence.size` | PVC size | `"1Gi"` |
| `persistence.storageClass` | PVC storage class | `"local-path"` |


## Usage

### Installation

```bash
helm install elchi-platform .
```

### Configuration

You can override values:

```yaml
# Global Values
global:
  namespace: "elchi-platform"
  mainURL: "your-domain.com"
  port: "443"
  tlsEnabled: true
  installMongo: true
  versions:
    - tag: v0.1.0-v0.13.4-envoy1.33.0
    - tag: v0.1.0-v0.13.4-envoy1.32.3
  mongodb:
    hosts: ""
    username: "elchi"
    password: "secure-password"
    database: "elchi"
  bigbang:
    grpcDefaultReplicas: 3
    restDefaultReplicas: 2

# Elchi Chart Values
elchi:
  enabled: true
  image:
    repository: spehlivan/elchi
    tag: latest
    pullPolicy: IfNotPresent
  service:
    type: ClusterIP
    port: 80
  resources:
    requests:
      memory: "128Mi"
      cpu: "100m"
    limits:
      memory: "256Mi"
      cpu: "200m"
  config:
    logging:
      level: "info"
      formatter: "text"
      reportCaller: "false"

# BigBang Chart Values
bigbang:
  image:
    repository: spehlivan/bigbang
    pullPolicy: IfNotPresent
  resources:
    rest:
      requests:
        memory: "256Mi"
        cpu: "150m"
      limits:
        memory: "512Mi"
        cpu: "250m"
    grpc:
      requests:
        memory: "256Mi"
        cpu: "150m"
      limits:
        memory: "512Mi"
        cpu: "250m"
  service:
    type: ClusterIP
    rest:
      port: 8099
    grpc:
      port: 18000
  config:
    logging:
      level: "info"
      formatter: "text"
      reportCaller: "false"

# Envoy Chart Values
envoy:
  enabled: true
  replicas: 4
  image:
    repository: envoyproxy/envoy
    tag: v1.33.0
    pullPolicy: IfNotPresent
  service:
    type: NodePort
    httpPort: 30080
    adminPort: 30081
  resources:
    requests:
      memory: "128Mi"
      cpu: "100m"
    limits:
      memory: "256Mi"
      cpu: "200m"

# MongoDB Chart Values
mongodb:
  enabled: true
  auth:
    rootPassword: "elchi"
    username: "elchi"
    password: "elchi"
    database: "elchi"
  persistence:
    enabled: true
    size: "1Gi"
    storageClass: "local-path"
```

Both approaches are valid and will work the same way. The second approach (complete structure) might be more readable when you need to override many values and want to keep the structure clear.

Then install/upgrade with:

```bash
helm install -f values.yaml elchi-platform .
# or
helm upgrade -f values.yaml elchi-platform .
```
