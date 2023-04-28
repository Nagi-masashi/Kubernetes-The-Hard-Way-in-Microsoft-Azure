# NGINX Ingress Controller Installation
```
wget https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.6.4/deploy/static/provider/cloud/deploy.yaml
```
Change validatingwebhookconfiguration.webhook.rules.apiVersions in deploy.yaml from [v1] to [v1beta1].
```
kubectl apply -f deploy.yaml
```
## Verification
```
kubectl run nginx --image=nginx
```
```
kubectl run web --image=httpd:2.4.48-alpine3.14
```
```
kubectl expose pod nginx --port=80 --type=NodePort --name=nginx-service
```
```
kubectl expose pod web --port=5000 --target-port=80 --type=NodePort --name=web-server-service
```
```
vi nginx-ingress.yaml
```
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  defaultBackend:
    service:
      name: nginx-service
      port:
        number: 80
  rules:
  - host: nginx.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx-service
            port:
              number: 80
  - host: web.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: web-server-service
            port:
              number: 5000
```
```
kubectl apply -f nginx-ingress.yaml
```
```
kubectl get svc -n ingress-nginx
```
output
```
NAME                                 TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.32.0.222   <pending>     80:30735/TCP,443:30277/TCP   26s
```
```
curl ${worker-instance-ip}:30735 -H "host:nginx.example.com"
```
```
curl ${worker-instance-ip}:30735 -H "host:web.example.com"
```
Port refers to the output ingress-nginx-controller port.

Confirm that the display changes depending on the host even though the destination is the same.

Prev: [Avi Deployment Guide for Microsoft Azure](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/12-Avi%20Deployment%20Guide%20for%20Microsoft%20Azure.md)
