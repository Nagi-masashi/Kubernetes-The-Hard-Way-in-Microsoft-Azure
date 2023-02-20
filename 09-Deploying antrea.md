# Deploying antrea 
```
wget https://raw.githubusercontent.com/antrea-io/antrea/main/build/yamls/antrea.yml
```
antrea.yamlのvalidatingwebhookconfiguration.WebHooks.name.rule.apiVersionsを["v1"]に変更(9ヶ所)

```
kubectl apply -f antrea.yaml
```
