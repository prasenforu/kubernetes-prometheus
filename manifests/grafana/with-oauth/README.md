## Setup standalone Grafana in Openshift 3.11




##### Step 1# Create a new project and set node selector for this project

```
oc new-project grafana

oc patch namespace security -p '{"metadata":{"annotations":{"openshift.io/node-selector":"sregion=infra"}}}'
```

##### Step 2# To add k8s-prometheus as a datasource in this standalone Grafana, you need to run gfollowing command to get credentials.

```
oc get secret grafana-datasources -n openshift-monitoring -o yaml | grep prometheus.yaml | cut -d':' -f2 | cut -d' ' -f2 | base64 -d > prom-datasources.yaml
```

##### Step 3# Create datasource configmap from file (prom-datasources.yaml) & create configmap resource

```oc create configmap --dry-run grafana-datasources --from-file=./prom-datasources.yaml --output yaml | tee datasources-cm.yaml```

##### Step 4# Create configmap resource

```oc create -f datasources-cm.yaml```

##### Step 5# Create Deployment

```oc new-app -f grafana-deployment-with-oauth-openshift.yaml```

##### Step 6# Get route URL

```oc get route |awk 'NR==2 {print $2}'```
