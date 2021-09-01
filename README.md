## Deploying a simple application in Auto Pilot cluster on GKE.
Hi guys, now I gonna show you how to deploy a auto-pilot cluster in Google Cloud Platform.

#### Prerequisites
1. An Account on GCP
2. Install Google cloud SDK (gcloud) in your machine (https://cloud.google.com/sdk/docs/install)
3. Install Kubectl in your machine (https://kubernetes.io/docs/tasks/tools/)
4. Install Docker in your machine (https://docs.docker.com/get-docker/)

#### Let's go!!
In your first time on GKE, you shall enable Kubernetes Engine on GCP. For more info (https://cloud.google.com/kubernetes-engine/docs/quickstart)

#### Creating a auto pilot cluster:
You can to create the cluster in the GUI of GCP.
1. In Kubernetes Engine click in CREATE CLUSTER
2. Choice GKE Autpilot and click in CONFIGURE button
3. Choice your VPC and Subnetwork (Pre-configured) or use default this one 🤣
4. Click in CREATE

![https://i.imgur.com/P9N47I9.png](https://i.imgur.com/P9N47I9.png) 
![https://i.imgur.com/nbXdtKX.png](https://i.imgur.com/nbXdtKX.png) 

#### Connecting kubectl in cluster
In your terminal, execute this command:
```console
$ gcloud container clusters get-credentials CLUSTER_NAME --zone us-central-c)
```
In flag --zone you shall to put the correct region of your cluster, for example us-central-c.

#### Generating Application Docker image:
Generate and upload of docker image in Google Cloud Docker HUB
To Generate, execute this command, where the project-id param you can get in URL of GCP "project="
```console
$ docker build -t gcr.io/[project-id]/simple-fastapi-example
```
To upload
```console
$ docker push gcr.io/[project-id]/simple-fastapi-example
```

#### Creating Loadbalancer
```console
$ kubectl apply -f .k8s/service.yaml
```
```console
$ kubectl get services
```
Output
```console
NAME          TYPE           CLUSTER-IP   EXTERNAL-IP    PORT(S)       AGE
app-service   LoadBalancer   x.x.x.x      192.168.0.1    80:30705/TCP  3s
kubernetes    ClusterIP      x.x.x.x      <none>         443/TCP       2s
```


#### 😆 Finally!! Deploying Application 🚀
```console
$ kubectl apply -f .k8s/deployment.yaml
```

For testing, you can access by browser the Loadbalancer external IP, for example 192.168.0.1

#### Main AutoPilot characteristics
1. GKE provisions and manages the cluster's underlying infrastructure, including nodes and node pools, giving you an optimized cluster with a hands-off experience
2. Auto Vertical Scalling 
3. Basically you need just to execute kubectl commands🤘🏻

#### Horizontal Pod Autoscaller (HPA)
The HPA (Horizontal Pod Autoscaler) is the responsible for managing the pod replication, through of a configuration in the hpa.yaml, you can install a metric collector to get the most suitable configuguration for your Autoscaller.

```console
$ kubectl apply -f .k8s/hpa.yaml
```

🚨🚨 Warning, for your HPA working correctly, you shall to configure some resource limit in your deployment if not an overload can occur.

#### Next time!!! 👍🏻
I will to speak more about the configs contained the deployment.yaml, service.yaml and hpa.yaml. Also will speak about set resource limits.

