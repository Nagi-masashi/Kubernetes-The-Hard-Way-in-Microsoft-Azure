# Octant and antrea-octant-plugin installation
```
wget https://github.com/antrea-io/antrea/blob/main/build/yamls/antrea-octant.yml
```
admin.kubeconfig のserverをkubernetes_public_ipに変える(新しくoctant.kubeconfigとした)
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
ブラウザでlocalhost:${access_port}を開く