## GCP-Test Guide

### Setup Kubernetes Engine
Visit https://console.cloud.google.com/kubernetes (or https://console.cloud.google.com/ and click on Kubernetes Engine.
Click Create Cluster - This failed for me with the error 'Failed to load'.
I then clicked on Deploy and selected the default (nginx:latest) image and settings. This created the kubernetes cluster and deployed aforementioned image.
After this I was able to create further clusters - may have been a timing issue. I destroyed this cluster.

#### Create a Kubernetes Cluster:
Setup with the following settings:
* Standard Cluster
* Name: standard-cluster-1 (left default name for this test)
* Location Type: Left this as Zonal - (I chose europe-west1-b)
* I left all the rest of the settings as default.
Click Create

### Setup gcloud with kubectl on local machine

First gcloud is needed. I followed the docs at https://cloud.google.com/sdk/docs/quickstarts for installation.
Then installed kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/

```
# gcloud container clusters get-credentials [clustername] --zone europe-west1-b --project [projectname]
```

Optional: I have a preference to use kubens to easily switch between namespaces (which also provides kubctx for switching between multiple clusters): https://github.com/ahmetb/kubectx
You can append `--namespace test` onto the kubectl command if you don't have/wish to use kubens
```
# kubectl create namespace test
# kubens test
```

### Setup Application

For simplicity I used the mysql manifests from https://kubernetes.io/docs/tasks/run-application/run-single-instance-stateful-application/ that I've modified to add secrets
In a production environment you would want to carefully store the secrets, not in this repo as I have done (purely for demonstration purposes).

Let's deploy the webserver using kubectl:
```
Create the secret:
# kubectl apply -f secrets/mysqlpass.yml
# kubectl apply -f manifests/mysql-deployment.yml
# kubectl apply -f manifests/wordpress-deployment.yml
# kubectl get pods
wait for them all to be available
# gcloud compute firewall-rules list
Find the newly created rule and using the name
# gcloud compute firewall-rules update [k8s-fw-rulename] --source-ranges=[mypublicip]/32
```

Now you (and you alone) should be able to visit the website instance.
