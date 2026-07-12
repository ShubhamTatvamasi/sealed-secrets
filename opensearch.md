# OpenSearch

Create a cookie:
```bash
COOKIE=$(openssl rand -base64 32 | tr -dc 'A-Za-z0-9' | head -c 32)
PASSWORD=$(htpasswd -bnBC 12 "" 'admin' | cut -d: -f2)
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
