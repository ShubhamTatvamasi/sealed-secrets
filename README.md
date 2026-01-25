# sealed-secrets

Add Helm repo:
```
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets/
```

Install Sealed Secrets
```bash
helm install sealed-secrets sealed-secrets/sealed-secrets \
  --namespace sealed-secrets \
  --create-namespace
```
