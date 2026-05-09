# TLS

Create a secret:
```bash
kubectl create secret tls shubhamtatvamasi-tls \
  --cert=fullchain.cer \
  --key=k8s.shubhamtatvamasi.com.key
  --dry-run=client -o yaml > /tmp/shubhamtatvamasi-tls.yaml
```
