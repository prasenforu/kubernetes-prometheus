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

#### Grafana Variable Regex with exclude

```/^(?!openshift|kube|default|management|grafana).*/```
