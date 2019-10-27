## GCP-Test Guide

### Setup Kubernetes Engine
Visit https://console.cloud.google.com/kubernetes (or https://console.cloud.google.com/ and click on Kubernetes Engine.
Click Create Cluster - This failed for me with the error 'Failed to load'.
I then clicked on Deploy and selected the default (nginx:latest) image and settings. This created the kubernetes cluster and deployed aforementioned image.
(Then I was able to create further clusters - may have been a timing issue)

#### Create a Kubernetes Cluster:
Setup with the following settings:
* Standard Cluster
* Name: standard-cluster-1 (left default name for this test)
* Location Type: Left this as Zonal - (I chose europe-west1-b)
* I left all the rest of the settings as default.
Click Create

### Setup gcloud with kubectl on local machine

First gcloud is needed. I followed the docs at https://cloud.google.com/sdk/docs/quickstarts for installation
THen installed kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/

```
# gcloud container clusters get-credentials [clustername] --zone europe-west1-b --project [projectname]
```

Optional: I have a preference to use kubens to easily switch between namespaces (which also provides kubctx for switching between multiple clusters): https://github.com/ahmetb/kubectx
You can append `--namespace test` onto the kubectl command
```
# kubectl create namespace test
# kubens test
```

### Setup Jenkins
