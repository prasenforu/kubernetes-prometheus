## Setup standalone Grafana in Openshift 3.11





##### Step 1# To add k8s-prometheus as a datasource in this standalone Grafana, you need to run gfollowing command to get credentials.

```
oc get secret grafana-datasources -n openshift-monitoring -o yaml | grep prometheus.yaml | cut -d':' -f2 | cut -d' ' -f2 | base64 -d > prom-datasources.yaml
```

##### Step 2# Create datasource configmap from file (prom-datasources.yaml) & create configmap resource

```
oc create configmap --dry-run grafana-datasources --from-file=./prom-datasources.yaml --output yaml | tee datasources-cm.yaml
```

##### Step 3# Create configmap resource

```
oc create -f datasources-cm.yaml
```

##### Step 4# Create ServiceAccount resource

```
oc create -f datasources-cm.yaml
```

##### Step 4# Create Deployment
```
oc create -f datasources-cm.yaml
```

##### Step 5# Create Service resource

```
oc create -f grafana.svc.yaml
```

##### Step 5# Create route resource

```
oc expose service/grafana
```
