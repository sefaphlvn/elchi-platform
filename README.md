[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/elchi-platform)](https://artifacthub.io/packages/search?repo=elchi-platform)

# Elchi Platform Helm Charts

This repository contains Helm charts for deploying the Elchi Platform components.

## Available Versions
> Syntax: `<bigbangVersion>`-`<goControlPlaneVersion>`-`<envoyVersion>`

- `v0.1.0-v0.13.4-envoy1.33.2`
- `v0.1.0-v0.13.4-envoy1.33.0`
- `v0.1.0-v0.13.4-envoy1.32.3`

## Global Values

| Parameter | Description | Default |
|-----------|-------------|---------|
| `global.namespace` | Namespace where all components will be deployed | `"elchi-platform"` |
| `global.mainURL` | Base URL for the all components | `""` |
| `global.port` | Port for the BigBang API. If empty, uses 80/443 based on TLS | `""` |
| `global.tlsEnabled` | Whether to use HTTPS | `false` |
| `global.installMongo` | Whether to use self-hosted MongoDB | `true` |
| `global.versions` | List of BigBang versions to deploy | `[v0.1.0-v0.13.4-envoy1.33.0, v0.1.0-v0.13.4-envoy1.32.3]` |
| `global.mongodb.hosts` | MongoDB connection hosts (comma-separated for replica set) | `""` |
| `global.mongodb.username` | MongoDB username | `"elchi"` |
| `global.mongodb.password` | MongoDB password | `"elchi"` |
| `global.mongodb.database` | MongoDB database name | `"elchi"` |
| `global.mongodb.scheme` | Connection scheme (mongodb or mongodb+srv) | `""` |
| `global.mongodb.port` | MongoDB connection port | `""` |
| `global.mongodb.replicaset` | Replica set name (if using replica set) | `""` |
| `global.mongodb.timeoutMs` | Connection timeout in milliseconds | `""` |
| `global.mongodb.tlsEnabled` | Enable TLS connection to MongoDB | `""` |
| `global.mongodb.authSource` | Authentication source database (e.g., admin) | `""` |
| `global.bigbang.grpcDefaultReplicas` | Default replicas for gRPC services | `1` |
| `global.bigbang.restDefaultReplicas` | Default replicas for REST services | `1` |

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
| `replicas` | Number of Elchi replicas | `3` |
| `image.repository` | Elchi image repository | `"spehlivan/elchi"` |
| `image.tag` | Elchi image tag | `"latest"` |
| `image.pullPolicy` | Image pull policy | `"IfNotPresent"` |
| `service.type` | Kubernetes service type | `"ClusterIP"` |
| `service.port` | Service port | `80` |
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
| `resources.requests.memory` | Memory request | `"128Mi"` |
| `resources.requests.cpu` | CPU request | `"100m"` |
| `resources.limits.memory` | Memory limit | `"256Mi"` |
| `resources.limits.cpu` | CPU limit | `"200m"` |

## MongoDB Chart Values

| Parameter | Description | Default |
|-----------|-------------|---------|
| `persistence.enabled` | Enable persistence | `true` |
| `persistence.size` | PVC size | `"1Gi"` |
| `persistence.storageClass` | PVC storage class | `"local-path"` |


## Usage

### Installation

```bash
helm install my-elchi-platform elchi-platform/elchi-platform --version 0.1.0
```

### Configuration

You can override values:

```yaml
# Global Values
global:
  namespace: "elchi-platform"
  mainURL: "your-domain.com"
  port: ""
  tlsEnabled: true
  installMongo: true
  versions:
    - tag: v0.1.0-v0.13.4-envoy1.33.2
    - tag: v0.1.0-v0.13.4-envoy1.32.3
  mongodb:
    username: "elchi"
    password: "elchi"
    database: "elchi"
  bigbang:
    grpcDefaultReplicas: 1
    restDefaultReplicas: 1
```

This approach (complete structure) might be more readable when you need to override many values and want to keep the structure clear.

Then install/upgrade with:

```bash
helm install -f values.yaml my-elchi-platform elchi-platform/elchi-platform
# or
helm upgrade -f values.yaml my-elchi-platform elchi-platform/elchi-platform
```

```bash
helm install my-elchi-platform elchi-platform/elchi-platform \
  --version 0.1.0 \
  --set-string global.mainURL="elchi.example.com" \
  --set-string global.port="23456"
```
