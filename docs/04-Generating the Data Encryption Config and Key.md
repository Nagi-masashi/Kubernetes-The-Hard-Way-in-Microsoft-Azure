# Generating the Data Encryption Config and Key
## The Encryption Key
```
head -c 32 /dev/urandom | base64
```
出力されたものを${ENCRYPTION_KEY}とする

## The Encryption Config File
```
vi encryption-config.yaml
```
```
kind: EncryptionConfig
apiVersion: v1
resources:
  - resources:
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: ${ENCRYPTION_KEY}
      - identity: {}
```

controller_instance = [k8s-dev-controller-0, k8s-dev-controller-1, k8s-dev-controller-2,…]
```
scp -i <pem file path> encryption-config.yaml ${instance}:~/
```

Next: [Bootstrapping the etcd Cluster](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/05-Bootstrapping%20the%20etcd%20Cluster.md)  
Prev: [Generating Kubernetes Configuration Files for Authentication](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/03-Generating%20Kubernetes%20Configuration%20Files%20for%20Authentication.md)
