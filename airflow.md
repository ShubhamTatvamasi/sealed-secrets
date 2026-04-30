# Airflow

### Git Token

Setup Fine-grained personal access tokens:
```bash
export GITHUB_TOKEN=<gh-token>
```

Create `git-credentials` secret:
```
kubectl create secret generic git-credentials \
  --from-literal=GIT_SYNC_USERNAME=git \
  --from-literal=GIT_SYNC_PASSWORD=$GITHUB_TOKEN \
  --from-literal=GITSYNC_USERNAME=git \
  --from-literal=GITSYNC_PASSWORD=$GITHUB_TOKEN \
  --dry-run=client -o yaml > /tmp/git-credentials.yaml
```

Create `git-credentials` sealed-secret:
```bash
kubeseal \
  --namespace airflow \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --format yaml \
  < /tmp/git-credentials.yaml > /tmp/git-credentials-sealedsecret.yaml
```
> use `--scope cluster-wide` if you can secret to be deployed on any namespace.


Apply sealed-secret:
```bash
kubectl apply -f /tmp/git-credentials-sealedsecret.yaml
```

---

### Airflow Connections

Create `airflow-connections` secret:
```bash
kubectl create secret generic airflow-connections \
  --from-literal=AIRFLOW_CONN_MY_POSTGRES="postgresql://postgres:postgres@postgres:5432/postgres" \
  --dry-run=client -o yaml > /tmp/airflow-connections.yaml
```
> Connection ID: `my_postgres`

Create `airflow-connections` sealed-secret:
```bash
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/airflow-connections.yaml > /tmp/airflow-connections-sealedsecret.yaml
```

Apply sealed-secret:
```bash
kubectl apply -f /tmp/airflow-connections-sealedsecret.yaml
```
