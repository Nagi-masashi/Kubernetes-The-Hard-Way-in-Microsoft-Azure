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
