# kube-deploy Helm Chart

A Helm chart for Kubernetes deployment automation.

## Description

This Helm chart provides a standardized way to deploy applications to Kubernetes clusters with configurable options for deployment, services, ingress, autoscaling, and more.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+

## Installation

### Add the repository (if applicable)

```bash
helm repo add kube-deploy https://charts.example.com
helm repo update
```

### Install the chart

```bash
# Install with default values
helm install my-release kube-deploy/kube-deploy

# Install with custom values
helm install my-release kube-deploy/kube-deploy -f custom-values.yaml

# Install with command-line overrides
helm install my-release kube-deploy/kube-deploy \
  --set image.repository=my-app \
  --set image.tag=v1.0.0
```

## Configuration

The following table lists the configurable parameters of the kube-deploy chart and their default values.

### Image Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | Container image repository | `nginx` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `image.tag` | Image tag (overrides appVersion) | `""` |
| `imagePullSecrets` | Image pull secrets | `[]` |

### Deployment Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `nameOverride` | Override chart name | `""` |
| `fullnameOverride` | Override full name | `""` |

### Service Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `service.type` | Kubernetes service type | `ClusterIP` |
| `service.port` | Service port | `80` |

### Ingress Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `ingress.enabled` | Enable ingress | `false` |
| `ingress.className` | Ingress class name | `""` |
| `ingress.annotations` | Ingress annotations | `{}` |
| `ingress.hosts` | Ingress hosts configuration | See values.yaml |
| `ingress.tls` | Ingress TLS configuration | `[]` |

### Autoscaling Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `autoscaling.enabled` | Enable HPA | `false` |
| `autoscaling.minReplicas` | Minimum replicas | `1` |
| `autoscaling.maxReplicas` | Maximum replicas | `100` |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization | `80` |
| `autoscaling.targetMemoryUtilizationPercentage` | Target memory utilization | Not set |

### Resources Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `resources.limits` | Resource limits | `{}` |
| `resources.requests` | Resource requests | `{}` |

### Security Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `serviceAccount.create` | Create service account | `true` |
| `serviceAccount.automount` | Automount service account token | `true` |
| `serviceAccount.annotations` | Service account annotations | `{}` |
| `serviceAccount.name` | Service account name | `""` |
| `podSecurityContext` | Pod security context | `{}` |
| `securityContext` | Container security context | `{}` |

### Other Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `podAnnotations` | Pod annotations | `{}` |
| `podLabels` | Pod labels | `{}` |
| `nodeSelector` | Node selector | `{}` |
| `tolerations` | Tolerations | `[]` |
| `affinity` | Affinity rules | `{}` |
| `volumes` | Additional volumes | `[]` |
| `volumeMounts` | Additional volume mounts | `[]` |

## Examples

### Deploy with custom image

```yaml
# values-custom.yaml
image:
  repository: myregistry/myapp
  tag: "1.2.3"
  pullPolicy: Always

replicaCount: 3

service:
  type: LoadBalancer
  port: 8080
```

```bash
helm install my-app kube-deploy/kube-deploy -f values-custom.yaml
```

### Enable ingress with TLS

```yaml
# values-ingress.yaml
ingress:
  enabled: true
  className: nginx
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
  hosts:
    - host: myapp.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: myapp-tls
      hosts:
        - myapp.example.com
```

```bash
helm install my-app kube-deploy/kube-deploy -f values-ingress.yaml
```

### Enable autoscaling

```yaml
# values-hpa.yaml
autoscaling:
  enabled: true
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 70
  targetMemoryUtilizationPercentage: 80

resources:
  requests:
    cpu: 100m
    memory: 128Mi
  limits:
    cpu: 500m
    memory: 512Mi
```

```bash
helm install my-app kube-deploy/kube-deploy -f values-hpa.yaml
```

## Upgrading

```bash
# Upgrade to new version
helm upgrade my-release kube-deploy/kube-deploy

# Upgrade with new values
helm upgrade my-release kube-deploy/kube-deploy -f new-values.yaml
```

## Uninstalling

```bash
helm uninstall my-release
```

## Testing

```bash
# Dry-run installation
helm install my-release kube-deploy/kube-deploy --dry-run --debug

# Template rendering
helm template my-release kube-deploy/kube-deploy

# Lint chart
helm lint .
```

## Development

### Local testing

```bash
# Install from local directory
helm install test-release ./kube-deploy

# Package chart
helm package ./kube-deploy

# Install from package
helm install test-release kube-deploy-0.1.0.tgz
```

## Contributing

Contributions are welcome! Please submit pull requests or open issues for bugs and feature requests.

## License

Copyright 2025 Vitor Gomes

## Maintainers

- Vitor Gomes (vitorafgomes@users.noreply.github.com)