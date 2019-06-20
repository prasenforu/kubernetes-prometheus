## Setup standalone Grafana in Openshift 3.11





##### 1# To add k8s-prometheus as a datasource in this standalone Grafana, you need to run gfollowing command to get credentials.

```
oc get secret grafana-datasources -n openshift-monitoring -o yaml | grep prometheus.yaml | cut -d':' -f2 | cut -d' ' -f2 | base64 -d > prom-datasources.yaml
```

##### Create datasource configmap from file (prom-datasources.yaml) & create configmap resource

```
oc create configmap --dry-run grafana-datasources --from-file=./prom-datasources.yaml --output yaml | tee datasources-cm.yaml
```

##### Create configmap resource

```
oc create -f datasources-cm.yaml
```
