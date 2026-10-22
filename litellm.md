# litellm

```
LITELLM_API_KEY=sk-...
```

```bash
kubectl create secret generic open-webui-secrets \
  --from-literal=litellm-api-key=${LITELLM_API_KEY} \
  --dry-run=client -o yaml > /tmp/open-webui-secrets.yaml
```


```bash
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/open-webui-secrets.yaml > /tmp/open-webui-secrets-sealedsecret.yaml
```
