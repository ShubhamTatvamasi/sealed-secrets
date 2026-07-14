# airflow connections

Create `connections.yaml` file:
```bash
vim connections.yaml
```

```yaml
postgres_default:
  conn_type: postgres
  description: Airflow Postgres
  extra: null
  host: postgres-rw-pooler.cnpg-system
  login: airflow
  password: airflow
  port: 5432
  schema: airflow
```


```
kubectl create secret generic airflow-connections-import \
  --from-file=connections.yaml=connections.yaml \
  --dry-run=client -o yaml > /tmp/airflow-connections-import.yaml
```

verify
```bash
cat /tmp/airflow-connections-import.yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```

```
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/airflow-connections-import.yaml > /tmp/airflow-connections-import-sealedsecret.yaml
```

  
