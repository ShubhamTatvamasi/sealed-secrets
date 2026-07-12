# OpenSearch

Create a cookie:
```bash
COOKIE=$(openssl rand -base64 32 | tr -dc 'A-Za-z0-9' | head -c 32)
```

Create a secret:
```
kubectl create secret generic opensearch-admin-credentials \
  --from-literal=username=admin \
  --from-literal=password=admin \
  --from-literal=cookie="$COOKIE" \
  --dry-run=client -o yaml > /tmp/opensearch-admin-credentials.yaml
```

Verify:
```bash
cat /tmp/opensearch-admin-credentials.yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```

Seal secret:
```
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/opensearch-admin-credentials.yaml > /tmp/opensearch-admin-credentials-sealedsecret.yaml
```

---

### Airflow


Get the real OpenSearch admin password:
```bash
PASS=$(kubectl -n opensearch get secret opensearch-admin-credentials -o jsonpath='{.data.password}' | base64 -d)
```

create a secret
```bash
kubectl create secret generic airflow-opensearch-connection \
  --from-literal=connection="$(printf "https://admin:%s@opensearch-cluster-master.opensearch:9200" "$PASS")" \
  --dry-run=client -o yaml > /tmp/airflow-opensearch-connection.yaml
```

verify
```bash
cat /tmp/airflow-opensearch-connection.yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```

seal it:
```bash
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/airflow-opensearch-connection.yaml > /tmp/airflow-opensearch-connection-sealedsecret.yaml
``
