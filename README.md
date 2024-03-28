# Nokia_Openshift_demo
A java application having a tomcat webserver and mysql database pods using namespace, configMap, secret, persistentVolume, service, label, selector and replicaset.

Pre-requisites:
---------------
1. Install 'kubectl'
2. Install 'minikube' or 'openshift'
3. Before working on this project make sure you have setup single node cluster and it is running.

Pod creation & project setup
----------------------------
Go to the directory where you have kept all your yaml files.

1. 1st create the images out of the Dockerfiles `Dockerfile-mysql` & `Dockerfile-tomcat`.
```
docker build -t mysql:1.1 -f Dockerfile-mysql .
docker build -t loginwebapp:1.2 -f Dockerfile-tomcat .
```
2. Create a namespace where you want to run your pod
```
kubectl create -f namespaceDefinition.yml
```
3. Create configMap
```
kubectl create -f configmap-mysql.yml
```

4. Create secret to secure the DB credentials
```
kubectl create -f secret-mysql.yml
```

5. Create *persistent volume* and *persistent volume claim*
```
kubectl create -f mysql-pv.yml
```

6. Now create pods using *replicaset*
**Note:** The service and replicaset are added in one yaml file
```
kubectl create -f replicasetDefinition-tomcat.yml
kubectl create -f replicasetDefinition-mysqldb.yml
```

At this point your pods will be up and running.

**Check pods and the services.**
```
Ranjits-MacBook-Air:Desktop ranjitswain$ kubectl get all --namespace=dev
NAME                             READY   STATUS    RESTARTS   AGE
pod/mysqldb-replicaset-w5tgg     1/1     Running   6          6d12h
pod/tomcatweb-replicaset-kn8bz   1/1     Running   6          6d12h
pod/tomcatweb-replicaset-tdj4q   1/1     Running   6          6d12h

NAME                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/mysqldb-service   ClusterIP   None             <none>        3306/TCP         6d12h
service/tomcat-service    NodePort    10.105.128.104   <none>        8080:30008/TCP   6d12h

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/mysqldb-replicaset     1         1         1       6d12h
replicaset.apps/tomcatweb-replicaset   2         2         2       6d12h
```
***i.e. http://localhost:30008/LoginWebApp/***

*LoginWebApp* is your application name.

![alt text](https://github.com/ranjit4github/kubernetes_demo_project/blob/master/Screenshot%202020-02-20%20at%209.43.33%20PM.png)
