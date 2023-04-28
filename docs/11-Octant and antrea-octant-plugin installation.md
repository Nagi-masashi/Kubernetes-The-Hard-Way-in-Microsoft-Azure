# Octant and antrea-octant-plugin installation
```
wget https://raw.githubusercontent.com/antrea-io/antrea/main/build/yamls/antrea-octant.yml
```
Change server in admin.kubeconfig to kubernetes_public_ip (copy admin.kubeconfig and modify it to new octant.kubeconfig)
```
kubectl create secret generic octant-kubeconfig --from-file=admin.conf=octant.kubeconfig -n kube-system
```
```
kubectl apply -f antrea-octant.yml
```
## Verification
```
ssh -i ${ssh key} -fN -L ${access_port}:${web_server_ip}:${web_server_ip_port} azureuser@${bastion_ip}
```
Open localhost:${access_port} in your browser.

Next: [Avi Deployment Guide for Microsoft Azure](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/12-Avi%20Deployment%20Guide%20for%20Microsoft%20Azure.md)  
Prev: [Deploying the DNS Cluster Add-on](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/10-Deploying%20the%20DNS%20Cluster%20Add-on.md)
