# Helm

```bash
helm version --short

helm create example-release
helm template example-release example-app
helm upgrade example-release exmaple-app --values ./example-app/values.yaml

# --set replicas=1
helm show values flask-app
helm -n default get values flask-app
helm upgrade -f flask-values2.yaml flask-app flask-app --dry-run --debug --set replicas=1

```

## Charts

### bitnami/mysql

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami

helm install mysql-release bitnami/mysql --set auth.rootPassword="akhil123",primary.service.type=ClusterIP,primary.service.port=3306
```

+ Chart Info <https://artifacthub.io/packages/helm/bitnami/mysql>
