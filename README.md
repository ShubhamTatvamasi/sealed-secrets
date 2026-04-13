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

### Git Token

Setup Fine-grained personal access tokens:
```bash
export GITHUB_TOKEN=<gh-token>
```

Create `git-credentials` secret:
```
kubectl create secret generic git-credentials \
  --from-literal=GIT_SYNC_USERNAME=git \
  --from-literal=GIT_SYNC_PASSWORD=<GITHUB_TOKEN> \
  --from-literal=GITSYNC_USERNAME=git \
  --from-literal=GITSYNC_PASSWORD=<GITHUB_TOKEN> \
  --dry-run=client -o yaml > /tmp/git-credentials.yaml
```


---

#### Backup Private Key

```bash
SEALED_SECRET_PRIVATE_KEY_SECRET=$(kubectl get secrets \
  -l sealedsecrets.bitnami.com/sealed-secrets-key=active \
  -n sealed-secrets \
  -o jsonpath='{.items[0].metadata.name}')
```

```bash
kubectl get secret $SEALED_SECRET_PRIVATE_KEY_SECRET \
  -n sealed-secrets -o yaml > /tmp/sealed-secrets-key-backup.yaml
```

#### Restore Private Key

Delete new secret:
```bash
kubectl delete secret -n sealed-secrets \
  -l sealedsecrets.bitnami.com/sealed-secrets-key
```

Apply old secret:
```bash
kubectl apply -f /tmp/sealed-secrets-key-backup.yaml
```

Restart sealed-secrets controller:
```bash
kubectl rollout restart deployment sealed-secrets -n sealed-secrets
```




