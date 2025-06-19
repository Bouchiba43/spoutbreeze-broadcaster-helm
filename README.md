# SpoutBreeze Broadcaster Helm Chart

This Helm chart deploys the **SpoutBreeze Broadcaster** component to your Kubernetes cluster in the `moon` namespace.

---

##  Prerequisites

- [Helm](https://helm.sh/docs/intro/install/) 3.x installed
- A Kubernetes cluster with appropriate permissions
- A namespace named `moon` (create it if it doesn't exist):
  ```bash
  kubectl create namespace moon
  ```

##  Installation

To install the chart with the release name `spoutbreeze-broadcaster` into the `moon` namespace:

```bash
helm install spoutbreeze-broadcaster . -n moon
```

**Note:** Run the above command from the root directory of this Helm chart (where the `Chart.yaml` file is located).

##  Upgrade

To upgrade the release with new changes:

```bash
helm upgrade spoutbreeze-broadcaster . -n moon
```

##  Uninstall

To delete the Helm release and its associated resources:

```bash
helm uninstall spoutbreeze-broadcaster -n moon
```

##  Configuration

The following table lists the configurable parameters of the SpoutBreeze Broadcaster chart and their default values.

### Environment Variables

| Parameter | Description | Default |
|-----------|-------------|---------|
| `env[0].name` | Gin mode setting | `"release"` |
| `env[1].name` | Cluster IP address | `"` |
| `env[2].name` | Moon port configuration | `"` |
| `env[3].name` | Application port | `"1323"` |

### Deployment Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `4` |
| `image.repository` | Container image repository | `bouchibaahmed/spoutbreeze-broadcaster` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `image.tag` | Image tag | `"0.0.0.4"` |

### Service Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `service.type` | Kubernetes service type | `NodePort` |
| `service.port` | Service port | `1323` |

### Health Checks

| Parameter | Description | Default |
|-----------|-------------|---------|
| `livenessProbe.httpGet.path` | Liveness probe endpoint | `/health` |
| `readinessProbe.httpGet.path` | Readiness probe endpoint | `/readiness` |

### Autoscaling (Disabled by Default)

| Parameter | Description | Default |
|-----------|-------------|---------|
| `autoscaling.enabled` | Enable horizontal pod autoscaling | `false` |
| `autoscaling.minReplicas` | Minimum number of replicas | `1` |
| `autoscaling.maxReplicas` | Maximum number of replicas | `100` |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization | `80` |

##  Customization

You can override default values by creating a custom `values.yaml` file:

```yaml
# custom-values.yaml
replicaCount: 2

env:
  - name: GIN_MODE
    value: "debug"
  - name: CLUSTER_IP
    value: "your-cluster-ip"
  - name: MOON_PORT_4444
    value: "your-port"
  - name: PORT
    value: "1323"

service:
  type: ClusterIP
  port: 8080

resources:
  limits:
    cpu: 500m
    memory: 512Mi
  requests:
    cpu: 250m
    memory: 256Mi
```

Then install with your custom values:

```bash
helm install spoutbreeze-broadcaster . -n moon -f custom-values.yaml
```

##  Health Checks

The application exposes the following health check endpoints:

- **Liveness Probe**: `GET /health` - Checks if the application is running
- **Readiness Probe**: `GET /readiness` - Checks if the application is ready to serve traffic

## 🔍 Troubleshooting

### Check Pod Status

```bash
kubectl get pods -n moon -l app.kubernetes.io/name=spoutbreeze-broadcaster
```

### View Pod Logs

```bash
kubectl logs -n moon -l app.kubernetes.io/name=spoutbreeze-broadcaster
```

### Describe Service

```bash
kubectl describe service -n moon spoutbreeze-broadcaster
```

### Port Forward for Local Testing

```bash
kubectl port-forward -n moon service/spoutbreeze-broadcaster 8080:1323
```

Then access the application at `http://localhost:8080`

##  Default Resource Configuration

- **Replicas**: 4 pods for high availability
- **Service Type**: NodePort for external access
- **Image**: `bouchibaahmed/spoutbreeze-broadcaster:0.0.0.4`
- **Port**: Application runs on port 1323
- **Namespace**: Deploys to `moon` namespace

##  Security Notes

- The chart creates a service account by default
- Pod security contexts are configurable but not set by default
- Image pull secrets can be configured if using private registries

##  Additional Resources

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Helm Documentation](https://helm.sh/docs/)
- [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)
