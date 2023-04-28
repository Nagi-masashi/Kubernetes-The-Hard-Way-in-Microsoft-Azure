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
