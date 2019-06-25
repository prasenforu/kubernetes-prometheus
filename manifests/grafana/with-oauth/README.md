## Setup standalone Grafana in Openshift 3.11 with oauth




##### Step 1# Create a new project and set node selector for this project

```
oc new-project grafana

oc patch namespace grafana -p '{"metadata":{"annotations":{"openshift.io/node-selector":"region=infra"}}}'
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

##### Step 7# Create clusterrole to view dashboard for user

```oc create -f clusterrole-grafan-monitoring.yaml```

##### Step 8# Add user in clusterrole to view dashboard, before run this command make sure you users should added in cluster (htpasswd)

```oc adm policy add-cluster-role-to-user grafana-monitoring-view <U S E R S>```

### Backup & Restore of grafana DB

Bydefault grafana database (grafana.db) stores in /var/lib/grafana folder. You can take a copy of grafana database (grafana.db) in local system. 

##### Backup grafana database (grafana.db)

```
oc get pod
oc cp -c grafana <SOURCE GRAFANA POD>:/var/lib/grafana/grafana.db .
cp grafana.db grafana.db_<D A T E>
```

##### Restore grafana database (grafana.db)

```
oc get pod
oc cp grafana.db_<D A T E> -c grafana <DESTINATION GRAFANA POD>:/var/lib/grafana/
oc rsh -c grafana <DESTINATION GRAFANA POD>
cd /var/lib/grafana/
ls -l
cp grafana.db_<D A T E> grafana.db
```
