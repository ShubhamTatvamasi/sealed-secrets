# airflow fernet key


```
AIRFLOW_FERNET_KEY=$(python3 -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())")
```

```
kubectl create secret generic airflow-fernet-key \
  --from-literal=fernet-key="$AIRFLOW_FERNET_KEY" \
  --dry-run=client -o yaml > /tmp/airflow-fernet-key.yaml
```

```
cat /tmp/airflow-fernet-key.yaml | \
  yq '.data |= with_entries(.value |= @base64d)'
```

```
kubeseal \
  --controller-name=sealed-secrets \
  --controller-namespace=sealed-secrets \
  --scope cluster-wide \
  --format yaml \
  < /tmp/airflow-fernet-key.yaml > /tmp/airflow-fernet-key-sealedsecret.yaml
```

