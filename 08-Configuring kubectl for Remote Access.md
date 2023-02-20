# Configuring kubectl for Remote Access
bastionから行うことを推奨

## The Admin Kubernetes Configuration File
KUBERNETES_PUBLIC_ADDRESS=controller_loadbalancer(シングルノードの場合、controller_ip)

```
kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://${KUBERNETES_PUBLIC_ADDRESS}:6443
```
```
kubectl config set-credentials admin \
    --client-certificate=admin.pem \
    --client-key=admin-key.pem
```
```
kubectl config set-context kubernetes-the-hard-way \
    --cluster=kubernetes-the-hard-way \
    --user=admin
```
```
kubectl config use-context kubernetes-the-hard-way
```

## Verification
```
kubectl get nodes
```
output
```
NAME       STATUS   ROLES    AGE     VERSION
worker-0   Ready    <none>   2m35s   v1.24.0
```
