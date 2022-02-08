# keycloak-kubernetes

# Deploy Keycloak cluster locally

See [Prerequisites](PREREQUISITES.md) to make sure you have Kubernetes Dashboard, nginx-ingress, and bitnami helm repo configured.

If you have all the prerequisites, then you are just a few commands from running your first Keycloak cluster on Kubernetes:

```bash
# create dedicated namespace for our deployments
kubectl create ns keycloak
# deploy PostgreSQL cluster
helm install -n keycloak keycloak-db bitnami/postgresql-ha
# deploy Keycloak cluster
kubectl apply -n keycloak -f keycloak.yaml
# create HTTPS ingress for Keycloak
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout auth-tls.key -out auth-tls.crt -subj "/CN=auth.localtest.me/O=keycloak"
kubectl create secret -n keycloak tls auth-tls-secret --key auth-tls.key --cert auth-tls.crt
kubectl apply -n keycloak -f keycloak-ingress.yaml
```

Keycloak is now available at: https://auth.localtest.me.