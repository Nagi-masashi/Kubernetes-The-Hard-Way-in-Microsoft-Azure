# Installing the Client Tools
## Install CFSSL
```
wget -q --show-progress --https-only --timestamping \
  https://storage.googleapis.com/kubernetes-the-hard-way/cfssl/1.4.1/linux/cfssl \
  https://storage.googleapis.com/kubernetes-the-hard-way/cfssl/1.4.1/linux/cfssljson
```
```
chmod +x cfssl cfssljson
```
```
sudo mv cfssl cfssljson /usr/local/bin/
```
### Verification
Verify cfssl and cfssljson version 1.4.1 or higher is installed:
```
cfssl version
```
Output
```
Version: 1.4.1
Runtime: go1.12.12
```
```
cfssljson --version
```
Output
```
Version: 1.4.1
Runtime: go1.12.12
```

## Install kubectl
```
wget https://storage.googleapis.com/kubernetes-release/release/v1.24.0/bin/linux/amd64/kubectl
```
```
chmod +x kubectl
```
```
sudo mv kubectl /usr/local/bin/
```
### Verification
Verify kubectlversion 1.21.0 or higher is installed:
```
kubectl version --client
```
Output
```
Client Version: version.Info{Major:"1", Minor:"21", GitVersion:"v1.24.0", GitCommit:"cb303e613a121a29364f75cc67d3d580833a7479", GitTreeState:"clean", BuildDate:"2021-04-08T16:31:21Z", GoVersion:"go1.16.1", Compiler:"gc", Platform:"linux/amd64"}
```

Next: [Provisioning a CA and Generating TLS Certificates](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/02-Provisioning%20a%20CA%20and%20Generating%20TLS%20Certificates.md)  
Prev: [Prerequisites](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/00-Prerequisites.md)
