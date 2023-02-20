# Deploying the DNS Cluster Add-on
## The DNS Cluster Add-on
Deploy the coredns cluster add-on:
```
kubectl apply -f https://storage.googleapis.com/kubernetes-the-hard-way/coredns-1.8.yaml
```
## Verification
```
kubectl get pods -l k8s-app=kube-dns -n kube-system
```
output
```
NAME                       READY   STATUS    RESTARTS   AGE
coredns-8494f9c688-hh7r2   1/1     Running   0          10s
coredns-8494f9c688-zqrj2   1/1     Running   0          10s
```
```
kubectl run busybox --image=busybox:1.28 --command -- sleep 3600
```
```
kubectl get pods -l run=busybox
```
output
```
NAME      READY   STATUS    RESTARTS   AGE
busybox   1/1     Running   0          3s
```
```
kubectl exec -ti busybox -- nslookup kubernetes
```
ouotput
```
Server:    10.32.0.10
Address 1: 10.32.0.10 kube-dns.kube-system.svc.cluster.local

Name:      kubernetes
Address 1: 10.32.0.1 kubernetes.default.svc.cluster.local
```
