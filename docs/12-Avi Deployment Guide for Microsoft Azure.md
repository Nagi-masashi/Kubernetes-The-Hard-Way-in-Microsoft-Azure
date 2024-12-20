# Avi Deployment Guide for Microsoft Azure
## Avi Controller Instantiation
Select azureVM VMware NSX Advanced Load Balancer (Avi Networks) on the marketplace and build a VM.

Minimum memory 28GB, 8CPU required.

Skip authentication and open in web browser.

## Install helm
```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
```
```
sudo apt-get install apt-transport-https --yes
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
```
```
sudo apt-get update
```
```
sudo apt-get install helm
```
## Install Avi Kubernetes Operator
```
helm repo add ako https://projects.registry.vmware.com/chartrepo/ako
```
```
kubectl create ns avi-system
```
```
helm search repo
```
```
helm show values ako/ako --version 1.9.1> values.yaml
```
Versionが合うようにyamlを持ってくる

values.yamlの内容編集

clusterName: kubernetes-the-hard-way

cniPlugin: 'antrea'

vipNetworkList:

 - networkName: "network" ※avi_controllerで先に設定する
 
 - cidr: "10.0.0.0/16"
   
serviceType: "NodePort"

controllerVersion: '22.1.1'

controllerHost: kubernetes Public IP

avicredentials:

  username: 'admin'
  
  password: 'Password1234'
  
Avi controllerの設定に合うように編集する

変更あればserviceEngineGroupNameなども変更する

```
helm install ako/ako --generate-name --version 1.9.1 -f values.yaml --set ControllerSettings.controllerHost=20.46.116.91 --set avicredentials.username=admin --set avicredentials.password=Password1234 --namespace=avi-system
```

Next: [NGINX Ingress Controller Installation](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/13-NGINX%20Ingress%20Controller%20Installation.md)  
Prev: [Octant and antrea-octant-plugin installation](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/11-Octant%20and%20antrea-octant-plugin%20installation.md)
