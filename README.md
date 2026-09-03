# sealed-secrets

Add Helm repo:
```
helm repo add sealed-secrets https://bitnami.github.io/sealed-secrets/
```

Install Sealed Secrets
```bash
helm upgrade -i sealed-secrets sealed-secrets/sealed-secrets \
  --namespace sealed-secrets \
  --create-namespace \
  --set keyrenewperiod=0
```

---

### Docker

Create docker secret:
```bash
kubectl create secret docker-registry harbor-registry \
  --namespace tekton-pipelines \
  --docker-server=harbor-registry.harbor.svc:5000 \
  --docker-username=robot$infrabooks+tekton \
  --docker-password=qaYSYnTYplhsg5hPMqryAfLwoA9ahwe8 \
  --dry-run=client -o yaml > /tmp/harbor-secret.yaml
```

Create sealed-secret:
```bash
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --format yaml \
  < /tmp/harbor-secret.yaml > /tmp/harbor-sealedsecret.yaml
```

Apply sealed-secret:
```bash
kubectl apply -f /tmp/harbor-sealedsecret.yaml
```

---



### Base64


Convert to base64:
```bash
cat /tmp/sealed-secrets-key-backup.yaml | base64 > /tmp/sealed-secrets-key-backup.yaml.b64
```

Convert back to yaml:
```bash
cat /tmp/sealed-secrets-key-backup.yaml.b64 | base64 -d > /tmp/sealed-secrets-key-backup.yaml
```

---

### FluxCD

Flux reconcile git-credentials:
```bash
flux reconcile kustomization git-credentials -n airflow
```
