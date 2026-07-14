# airflow connections

Create `connections.json` file:
```bash
vim connections.json
```

```json
{
  "postgres_default": {
    "description": "Airflow Postgres",
    "conn_type": "postgres",
    "login": "airflow",
    "password": "airflow",
    "host": "postgres-rw-pooler.cnpg-system",
    "port": 5432,
    "schema": "airflow",
    "extra": null
  }
}
```


```
kubectl create secret generic airflow-connections-import \
  --from-file=connections.json=connections.json \
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

  
