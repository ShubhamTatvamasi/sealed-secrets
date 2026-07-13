# airflow connections

```
kubectl create secret generic airflow-connections-import-v1 \
  --from-file=connections.json=connections.json \
  --dry-run=client -o yaml > /tmp/airflow-connections-import-v1.yaml
```

verify
```bash
cat /tmp/airflow-connections-import-v1.yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```

```
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/airflow-connections-import-v1.yaml > /tmp/airflow-connections-import-v1-sealedsecret.yaml
```

  
