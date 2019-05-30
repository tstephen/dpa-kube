HOWTO run Camunda with Postgres database
========================================

Pre-requisites
--------------
- Start minikube
  ```
  minikube start
  ```
- Tell kubectl the cluster to work with
  ```
  kubectl config use-context minikube
  ```

Start Postgres
--------------

- Run configuration files
  ```
  kubectl create -f cfg/postgres-configmap.yaml  # environment to override
  kubectl create -f cfg/postgres-storage.yaml    # persistent storage
  kubectl create -f cfg/postgres-deployment.yaml # container
  kubectl create -f cfg/postgres-service.yaml    # allow us to connect to it
  ```
  
- Check where minikube is running
  ```
  minikube status
  ```
  results in something like this, note the IP address:
  ```
  host: Running
  kubelet: Running
  apiserver: Running
  kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.106
  ```
- Find the local port the postgres container's port has been mapped to:
  ```
  kubectl get services
  ```
  in this case 30192.
  ```
  NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
  kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP          11m
  postgres     NodePort    10.105.133.102   <none>        5432:30192/TCP   115s
  ```
- Test connectivity to postgres (replace all values with your own)
  ```
  psql -h 192.168.99.106 -U postgresadmin --password -p 30192 postgresdb
  ```
  Provide the password specified in your `postgres-configmap.yaml`
  ```
  Password: 
  psql (11.1, server 10.4 (Debian 10.4-2.pgdg90+1))
  Type "help" for help.

  postgresdb=# 
  ```

Start Camunda process engine configured to talk to the Postgres just started
------------------------------------------------------------------------------

The Camunda container's environment documentation is [here](https://github.com/camunda/docker-camunda-bpm-platform/blob/master/README.md).

- Run configuration files
  ```
  kubectl create -f cfg/camunda-configmap.yaml  # environment to override
  kubectl create -f cfg/camunda-deployment.yaml # container
  kubectl create -f cfg/camunda-service.yaml    # allow us to connect to it
  ```

Connect to the Camunda apps
---------------------------

- Have minikube do the translation of host and port by typing `minikube service camunda`, which for example opens the default browser at http://192.168.99.106:31681.
- Add the application context in the browser address bar yourself: /camunda-welcome/
