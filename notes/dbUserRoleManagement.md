# Manage DB, user and roles
[>](https://access.crunchydata.com/documentation/postgres-operator/latest/tutorials/basic-setup/user-management)

```yaml
spec:
  users:
    - name: rhino
      databases:
        - zoo
      options: "CREATEDB CREATEROLE"
```

    export PG_CLUSTER_USER_SECRET_USER=hippo-pguser-antslift-user
    export PGUSER=$(    kubectl get secrets -n postgres-operator "${PG_CLUSTER_USER_SECRET_USER}" -o go-template='{{.data.user     | base64decode}}')
    export PGPASSWORD=$(kubectl get secrets -n postgres-operator "${PG_CLUSTER_USER_SECRET_USER}" -o go-template='{{.data.password | base64decode}}')
    export PGDATABASE=$(kubectl get secrets -n postgres-operator "${PG_CLUSTER_USER_SECRET_USER}" -o go-template='{{.data.dbname   | base64decode}}')
    psql -h localhost
        psql (14.8 (Homebrew), server 14.9)
        SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
        Type "help" for help.
        
        antslift_db=> \d

    export PG_CLUSTER_USER_SECRET_PG=hippo-pguser-postgres
    export PGUSER=$(    kubectl get secrets -n postgres-operator "${PG_CLUSTER_USER_SECRET_PG}" -o go-template='{{.data.user     | base64decode}}')
    export PGPASSWORD=$(kubectl get secrets -n postgres-operator "${PG_CLUSTER_USER_SECRET_PG}" -o go-template='{{.data.password | base64decode}}')
    export PGDATABASE=antslift_db
    psql -h localhost
