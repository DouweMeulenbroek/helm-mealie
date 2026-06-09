# Mealie Helm Chart

This chart installs **Mealie**, a self-hosted recipe manager.

## Installation

```bash
helm repo add mealie https://douwemeulenbroek.github.io/helm-mealie
helm repo update
helm install my-mealie mealie/mealie --version 1.1.1

Configuration

See values.yaml for configurable options such as:

    image.tag – Docker image version

    postgresql – Database configuration (uses the Bitnami PostgreSQL subchart)

Dependencies

This chart depends on PostgreSQL, which is included as a subchart from:

https://charts.bitnami.com/bitnami

Helm automatically handles it when you install the chart.
Usage

After installation, access Mealie using the service name configured in values.yaml:
```bash
kubectl get svc my-mealie
```
For further customization, edit values.yaml and upgrade:

helm upgrade my-mealie mealie/mealie -f values.yaml