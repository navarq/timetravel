# timetravel
Time travel movies example using Redhat's Openshift. 


Create a project for the instance

```{sh }
oc new-project timetravel
```

Create a new Postgres database 
```{sh }
oc new-app timetraveldb --name ttdb --param DATABASE_SERVICE_NAME=ttdb --param POSTGRESQL_DATABASE=timetravelmovies --param POSTGRESQL_USER=timetraveller --param POSTGRESQL_PASSWORD=itaintpossible
```

Note the above database will no longer exist after the openshift instance closes 


Run the following command to find where the database is running
```{sh }
oc get pods --selector name=ttdb
```

We need now to capture the name of the pod

```{sh name-of-pod}
POD=`oc get pods --selector name=ttdb -o custom-columns=NAME:.metadata.name --no-headers`;echo $POD
```

Remote shell into pod
```{sh }
oc rsh $POD
```

Now that you are in the container you can at this point run the database client for the database
if provided in the container.
```{sh }
psql timetravelmovies timetraveller
```
