# Provisioning a CA and Generating TLS Certificates
## Certificate Authority
```
{

cat > ca-config.json <<EOF
{
  "signing": {
    "default": {
      "expiry": "8760h"
    },
    "profiles": {
      "kubernetes": {
        "usages": ["signing", "key encipherment", "server auth", "client auth"],
        "expiry": "8760h"
      }
    }
  }
}
EOF

cat > ca-csr.json <<EOF
{
  "CN": "Kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "Kubernetes",
      "OU": "CA",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert -initca ca-csr.json | cfssljson -bare ca

}
```
Results:
```
ca-key.pem
ca.pem
```
## Client and Server Certificates
### The Admin Client Certificate
```
{

cat > admin-csr.json <<EOF
{
  "CN": "admin",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:masters",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  admin-csr.json | cfssljson -bare admin

}
```
Results:
```
admin-key.pem
admin.pem
```
### The Kubelet Client Certificates
worker_instance=[k8s-dev-worker-0,k8s-dev-worker-1,k8s-dev-worker-2,…]

EXTERNAL_IP=woker-instance_public_ip

INTERNAL_IP=worker-instance_private_ip
```
vi ${worker_instance}-csr.json
```
```
{
  "CN": "system:node:${worker_instance}",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:nodes",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
```
```
cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -hostname=${worker-instance},${EXTERNAL_IP},${INTERNAL_IP} -profile=kubernetes ${worker-instance}-csr.json | cfssljson -bare ${worker-instance}
```
Results:
```
${worker-instance}-key.pem
${worker-instance}.pem
```
### The Controller Manager Client Certificate
```
{

cat > kube-controller-manager-csr.json <<EOF
{
  "CN": "system:kube-controller-manager",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:kube-controller-manager",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-controller-manager-csr.json | cfssljson -bare kube-controller-manager

}
```
Results:
```
kube-controller-manager-key.pem
kube-controller-manager.pem
```
### The Kube Proxy Client Certificate
```
{

cat > kube-proxy-csr.json <<EOF
{
  "CN": "system:kube-proxy",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:node-proxier",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-proxy-csr.json | cfssljson -bare kube-proxy

}
```
Results:
```
kube-proxy-key.pem
kube-proxy.pem
```
### The Scheduler Client Certificate
```
{

cat > kube-scheduler-csr.json <<EOF
{
  "CN": "system:kube-scheduler",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "system:kube-scheduler",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  kube-scheduler-csr.json | cfssljson -bare kube-scheduler

}
```
Results:
```
kube-scheduler-key.pem
kube-scheduler.pem
```
### The Kubernetes API Server Certificate
KUBERNETES_PUBLIC_ADDRESS=controller_loadbalancer(シングルノードの場合、controller_ip)
```
cat > kubernetes-csr.json <<EOF
{
  "CN": "kubernetes",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "Kubernetes",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF
```
```
cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -hostname=10.32.0.1,(all_controller_ip),${KUBERNETES_PUBLIC_ADDRESS},127.0.0.1,kubernetes,kubernetes.default,kubernetes.default.svc,kubernetes.default.svc.cluster,kubernetes.svc.cluster.local -profile=kubernetes kubernetes-csr.json | cfssljson -bare kubernetes
```
Results:
```
kubernetes-key.pem
kubernetes.pem
```
### The Service Account Key Pair
```
{

cat > service-account-csr.json <<EOF
{
  "CN": "service-accounts",
  "key": {
    "algo": "rsa",
    "size": 2048
  },
  "names": [
    {
      "C": "US",
      "L": "Portland",
      "O": "Kubernetes",
      "OU": "Kubernetes The Hard Way",
      "ST": "Oregon"
    }
  ]
}
EOF

cfssl gencert \
  -ca=ca.pem \
  -ca-key=ca-key.pem \
  -config=ca-config.json \
  -profile=kubernetes \
  service-account-csr.json | cfssljson -bare service-account

}
```
Results:
```
service-account-key.pem
service-account.pem
```


### Distribute the Client and Server Certificates
worker_instance=[k8s-dev-worker-0,k8s-dev-worker-1,k8s-dev-worker-2,…]
```
scp ca.pem ${worker-instance}-key.pem ${worker-instance}.pem ${worker-instance}:~/
```
メモ：↑ここでChmodが必要<br>
controller_instance=[k8s-dev-controller-0,k8s-dev-controller-1,k8s-dev-controller-2,…]
```
scp ca.pem ca-key.pem kubernetes-key.pem kubernetes.pem \
    service-account-key.pem service-account.pem ${controller-instance}:~/
```
