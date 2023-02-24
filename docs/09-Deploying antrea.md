# Deploying antrea 
```
wget https://raw.githubusercontent.com/antrea-io/antrea/main/build/yamls/antrea.yml
```
antrea.yamlのvalidatingwebhookconfiguration.WebHooks.name.rule.apiVersionsを["v1"]に変更(9ヶ所)

```
kubectl apply -f antrea.yaml
```
## Traceflow User Guide
### Prerequisites
antrea-configのtraceflowを有効にする
agentのAntreaProxyも有効にする必要があります
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
