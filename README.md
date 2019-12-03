dpa-kube
========

Sample configurations to run open-source, cloud-native process platforms.

The general approach is to give instructions for `minikube` and kubectl running locally but of course the point of running under Kubernetes is that these same configurations can be used on production deployment platforms such as AWS, Azure and DigitalOcean. 

- To install minikube read [this](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- To install kubectl read [this](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

Sensitive information such as passwords are in configmaps, make sure to change them!

Files
-----

- cfg  <- Kubernetes configuration files
- docs <- HOWTOs for specific configurations

