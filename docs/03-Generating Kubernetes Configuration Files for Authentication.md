# Generating Kubernetes Configuration Files for Authentication
## The kubelet Kubernetes Configuration File
worker_instance = [k8s-dev-worker-0, k8s-dev-worker-1, k8s-dev-worker-2,…]
```
kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://${KUBERNETES_PUBLIC_ADDRESS}:6443 --kubeconfig=${worker_instance}.kubeconfig
```
```
kubectl config set-credentials system:node:${worker_instance} --client-certificate=${worker_instance}.pem --client-key=${worker_instance}-key.pem --embed-certs=true --kubeconfig=${worker_instance}.kubeconfig
```
```
kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:node:${worker_instance} --kubeconfig=${worker_instance}.kubeconfig
```
```
kubectl config use-context default --kubeconfig=${worker_instance}.kubeconfig
```
Results:
```
${worker_instance}.kubeconfig
```
## The kube-proxy Kubernetes Configuration File
KUBERNETES_PUBLIC_ADDRESS = controller_loadbalancer (シングルノードの場合はcontroller_ip)
```
kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://${KUBERNETES_PUBLIC_ADDRESS}:6443 --kubeconfig=kube-proxy.kubeconfig
```
```
kubectl config set-credentials system:kube-proxy \
    --client-certificate=kube-proxy.pem \
    --client-key=kube-proxy-key.pem \
    --embed-certs=true \
    --kubeconfig=kube-proxy.kubeconfig
```
```
kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-proxy \
    --kubeconfig=kube-proxy.kubeconfig
 ```
 ```
 kubectl config use-context default --kubeconfig=kube-proxy.kubeconfig
 ```
Results:
```
kube-proxy.kubeconfig
```
## The kube-controller-manager Kubernetes Configuration File
```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config set-credentials system:kube-controller-manager \
    --client-certificate=kube-controller-manager.pem \
    --client-key=kube-controller-manager-key.pem \
    --embed-certs=true \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-controller-manager \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config use-context default --kubeconfig=kube-controller-manager.kubeconfig
}
```
Results:
```
kube-controller-manager.kubeconfig
```
## The kube-scheduler Kubernetes Configuration File
```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config set-credentials system:kube-scheduler \
    --client-certificate=kube-scheduler.pem \
    --client-key=kube-scheduler-key.pem \
    --embed-certs=true \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-scheduler \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config use-context default --kubeconfig=kube-scheduler.kubeconfig
}
```
Results:
```
kube-scheduler.kubeconfig
```
## The admin Kubernetes Configuration File
```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.pem \
    --embed-certs=true \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=admin.kubeconfig

  kubectl config set-credentials admin \
    --client-certificate=admin.pem \
    --client-key=admin-key.pem \
    --embed-certs=true \
    --kubeconfig=admin.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=admin \
    --kubeconfig=admin.kubeconfig

  kubectl config use-context default --kubeconfig=admin.kubeconfig
}
```
Results:
```
admin.kubeconfig
```
## Distribute the Kubernetes Configuration Files
worker_instance = [k8s-dev-worker-0, k8s-dev-worker-1, k8s-dev-worker-2,…]
```
scp -i <pem file path> ${worker_instance}.kubeconfig kube-proxy.kubeconfig ${worker_instance}:~/
```

controller_instance = [k8s-dev-controller-0, k8s-dev-controller-1, k8s-dev-controller-2,…]
```
scp -i <pem file path> admin.kubeconfig kube-controller-manager.kubeconfig kube-scheduler.kubeconfig ${controller_instance}:~/
```
