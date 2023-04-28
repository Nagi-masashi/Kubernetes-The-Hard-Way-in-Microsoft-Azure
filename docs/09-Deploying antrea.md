# Deploying antrea 
```
wget https://raw.githubusercontent.com/antrea-io/antrea/main/build/yamls/antrea.yml
```
Change validatingwebhookconfiguration.WebHooks.rules.apiVersions in antrea.yaml to ["v1"] (9 places).

```
kubectl apply -f antrea.yml
```
## Traceflow User Guide
### Prerequisites
Enable traceflow in antrea-config in antrea.yml.

The agent's AntreaProxy must also be enabled.
```
apiVersion: v1
data:
  antrea-agent.conf: |
    featureGates:
    # Enable AntreaProxy which provides ServiceLB for in-cluster Services in antrea-agent.
    # It should be enabled on Windows, otherwise NetworkPolicy will not take effect on
    # Service traffic.
      AntreaProxy: true

    # Enable traceflow which provides packet tracing feature to diagnose network issue.
      Traceflow: true
```
```
antrea-controller.conf: |
    # FeatureGates is a map of feature names to bools that enable or disable experimental features.
    featureGates:
    # Enable traceflow which provides packet tracing feature to diagnose network issue.
      Traceflow: true
```
Apply again after modification.
```
kubectl apply -f antrea.yml
```

Next: [Deploying the DNS Cluster Add-on](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/10-Deploying%20the%20DNS%20Cluster%20Add-on.md)  
Prev: [Configuring kubectl for Remote Access](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/08-Configuring%20kubectl%20for%20Remote%20Access.md)
