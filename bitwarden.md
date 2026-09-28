# Bitwarden


```bash
kubectl get secret bitwarden-secret -n bitwarden -o yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```
