# OpenSearch

Create a secret:
```
kubectl create secret generic opensearch-admin-credentials \
  --from-literal=username=admin \
  --from-literal=password=admin \
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
