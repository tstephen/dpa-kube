HOWTO run Kubernetes on Digital Ocean
=====================================

Pre-requisites
--------------

Install:
 - `doctl`: [Ref](https://github.com/digitalocean/doctl#snap-supported-os)
 - `kubectl`
 - `helm`

All are available as snaps. Remember to 'connect' doctl to kubectl. Remember too to generate your Digital Ocean token and register it with doctl (`doctl auth init`)

Steps
-----

1. Create cluster - takes 4 minutes!

   ```
   doctl k8s cluster create f-cluster --region lon1 --count 1 --size s-2vcpu-4gb
   ```

   This includes connecting doctl to the newly created cluster but if you need to do it manually this is the [Ref](https://www.digitalocean.com/docs/kubernetes/how-to/connect-to-cluster/)
   TIP: grab the kubeconfig for an existing cluster using:

   ```
   doctl k8s cluster kubeconfig save <cluster-id|cluster-name>
   ```

2. Test your connectivity with the following command:

   ```
   kubectl cluster-info
   ```

3. Setup Helm on local (control) machine

   ```
   helm init
   ```

3. Add a helm repo (somewhere to find charts)
   [Ref](https://www.digitalocean.com/community/tutorials/how-to-install-software-on-kubernetes-clusters-with-the-helm-package-manager)

   ```
   helm repo add stable https://kubernetes-charts.storage.googleapis.com/
   ```

4. Setup 'Tiller' on cluster

   ```
   # create the tiller service account
   kubectl -n kube-system create serviceaccount tiller
   # bind the tiller serviceaccount to the cluster-admin role
   kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
   # init tiller on cluster
   helm init --service-account tiller
   ```

   Verify that Tiller is running

   ```
   kubectl get pods --namespace kube-system

5. Ready to go! Let's install dashboard
   [Ref](https://github.com/kubernetes/dashboard)

   ```
   helm install stable/kubernetes-dashboard --name dashboard-demo
   ```

   Check it worked

   ```
   helm list
   kubectl get services
   ```

6. Upgrade it, in this case just change its name

   ```
   helm upgrade dashboard-demo stable/kubernetes-dashboard --set fullnameOverride="dashboard"
   ```

7. Let's actually see the dashboard then!

    ```
    # create proxy to be able to access cluster from control machine
    kubectl proxy
    ```

   Then pop this into your browser:
   > http://localhost:8001/api/v1/namespaces/default/services/https:dashboard:/proxy/

If necessary, substitute your own service name and namespace for the highlighted portions. Instructions for actually using the dashboard are out of scope for this tutorial, but you can read the official Kubernetes Dashboard docs for more information.

8. Setup an ingress controller

   [Ref](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nginx-ingress-on-digitalocean-kubernetes-using-helm)

   Install the stable Nginx Ingress Controller on cluster with name and parameter set.

   ```
   helm install stable/nginx-ingress --name nginx-ingress --set controller.publishService.enabled=true
   ```

   Check status (takes a minute or so to get going):

   ```
   kubectl --namespace default get services -o wide -w nginx-ingress-controller
   ```

9. Setup database
   [Ref](https://severalnines.com/database-blog/using-kubernetes-deploy-postgresql)

   ```
   helm install stable/postgresql
   ```

   Follow instructions displayed by helm chart to find db host and password.

10. Edit flowable-configmap as necessary

11. Run all flowable yamls and wait a bit


12. Alternately....

    ```
    helm install --name f -f v.yaml flowable 
    ```

1. Clean up only flowable release

    ```
    helm delete f --purge 
    ```

1. Clean-up: delete entire cluster in one easy move!

   ```
   doctl k8s cluster delete f-cluster
   ```
