# sentry

Sentry.io deployment to Openshift

### Install using CLI

##### Install minio first if you do not have other S3 storage available:

1. modify pvc setup in pvc.yaml and storageclass.yaml
2. edit deployment.yaml access key and secret key (these can be loaded from secrets)

3. finally create all

```
cd minio
oc create -f . -R
```

check that minio is started correctly `oc get pods`


##### Installing sentry

1. stack.yaml contains lots of environment specific settings, please edit those first
2. create it `oc create -f stack.yaml`, wait that database is up and running you can check those `oc get pods`
3. find your postgresql container `oc get pods|grep postgre`
4. `oc rsh postgresql-95-centos7-1-2ccwd` and then write `psql` and insert command `CREATE EXTENSION IF NOT EXISTS citext;` 
5. Edit populator.yaml env variables with your own specific settings, then create it `oc create -f populator.yaml`
6. wait that populator will populate the db, you can follow the process `oc get pods |grep database-populator` and `oc logs database-populator`. This might take few minutes.
7. After the database-populator is Completed, write `oc delete pod database-populator`
8. try login in https://yoursentryurl 