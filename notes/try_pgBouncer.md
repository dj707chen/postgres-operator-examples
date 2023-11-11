
## Terminal 0
    # Create GKE cluster
    cd $HOME/repoAntslift/antslift-infra/aaCloudSetup/wip
    2-gke.sh

    # Deploy Crunchy Postgres for Kubernetes  
    cd $HOME/repoAntslift/antslift-infra/GKE/CrunchyDataPgOperator/nestedRepo/postgres-operator-examples
    kubectl apply -k kustomize/postgres-pgbouncer

## Terminal 1
    export PG_CLUSTER_PRIMARY_POD=$(kubectl get pod -n postgres-operator -o name \
        -l postgres-operator.crunchydata.com/cluster=hippo,postgres-operator.crunchydata.com/role=master)
    kubectl -n postgres-operator port-forward "${PG_CLUSTER_PRIMARY_POD}" 5432:5432

Or

    kubectl get services -n postgres-operator
        NAME              TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE
        hippo-ha          NodePort    10.8.15.38    <none>        5432:32000/TCP   38m
        hippo-ha-config   ClusterIP   None          <none>        <none>           38m
        hippo-pgbouncer   ClusterIP   10.8.11.228   <none>        5432/TCP         19m
        hippo-pods        ClusterIP   None          <none>        <none>           38m
        hippo-primary     ClusterIP   None          <none>        5432/TCP         38m
        hippo-replicas    ClusterIP   10.8.4.23     <none>        5432/TCP         38m
    kubectl -n postgres-operator port-forward svc/hippo-pgbouncer pgbouncer

## Terminal 2
    export PG_CLUSTER_USER_SECRET_NAME=hippo-pguser-hippo
    export PGUSER=$(    kubectl get secrets -n postgres-operator "${PG_CLUSTER_USER_SECRET_NAME}" -o go-template='{{.data.user     | base64decode}}')
    export PGPASSWORD=$(kubectl get secrets -n postgres-operator "${PG_CLUSTER_USER_SECRET_NAME}" -o go-template='{{.data.password | base64decode}}')
    export PGDATABASE=$(kubectl get secrets -n postgres-operator "${PG_CLUSTER_USER_SECRET_NAME}" -o go-template='{{.data.dbname   | base64decode}}')
    psql -h localhost

## Delete
    kubectl delete -k kustomize/postgres-pgbouncer
