![Welcome Banner](https://raw.githubusercontent.com/etechlearner/elastic-gke-logging/master/resource/Screenshot1.png)

## How To Set Up an Elasticsearch, Fluentd and Kibana (EFK) Logging Stack on Kubernetes

We’ll begin by configuring and launching a scalable Elasticsearch cluster, and then create the Kibana Kubernetes Service and Deployment. To conclude, we’ll set up Fluentd as a DaemonSet so it runs on every Kubernetes worker node.

This tutorial looks at how to spin up a single node Elasticsearch cluster along with Kibana and Fluentd on Kubernetes.

Dependencies:

Docker v18.09.1
Kubernetes v1.13.2
Elasticsearch v6.5.4
Kibana v6.5.4
Fluentd v1.3.2

### Prerequisites

Before you begin with this guide, ensure you have the following available to you:

A Kubernetes 1.10+ cluster with role-based access control (RBAC) enabled

Ensure your cluster has enough resources available to roll out the EFK stack, and if not scale your cluster by adding worker nodes. We’ll be deploying a 3-Pod Elasticsearch cluster (you can scale this down to 1 if necessary), as well as a single Kibana Pod. Every worker node will also run a Fluentd Pod. The cluster in this guide consists of 3 worker nodes and a managed control plane.
The kubectl command-line tool installed on your local machine, configured to connect to your cluster. You can read more about installing kubectl in the official documentation.
Once you have these components set up, you’re ready to begin with this guide.

### Step 1: Set up a GKE Cluster on Gcloud
When launching a GKE cluster to run Portworx, you need to ensure that the cluster is based on Ubuntu. Due to certain restrictions with GKE clusters based on Container-Optimized OS (COS), Portworx requires Ubuntu as the base image for the GKE Nodes.

The following command configures a 3-node GKE Cluster in zone ap-south-1-a. You can modify the parameters accordingly.

### Step 1 — Start configuration on cli 
```markdown
$ gcloud init
```

##### Create Cluster 
```markdown
$ gcloud container clusters create "gke-logging" \
--zone "asia-south1-a" \
--username "admin" \
--cluster-version "1.8.10-gke.0" \
--machine-type "n1-standard-1" \
--image-type "UBUNTU" \
--disk-type "pd-ssd" \
--disk-size "100" \
--num-nodes "3" \
--enable-cloud-logging \
--enable-cloud-monitoring \
--network "default" \
--addons HorizontalPodAutoscaling,HttpLoadBalancing,KubernetesDashboard
```

##### Start configuration on cli 
```markdown
$ gcloud container clusters get-credentials gke-logging --zone asia-south1-a  --project my-project
```

##### Verify configuration on cli 
```markdown
$ kubectl get nodes
```

### Step 2 — Creating a Namespace
```markdown
$ kubectl create namespace logging
```

### Step 3 — Creating a storage class for ELK stack on GKE
```markdown
$ kubectl create -f elastic-sc.yaml -n logging
```

### Step 4 — Creating the Elasticsearch StatefulSet on GKE
```markdown
$ kubectl create -f elastic-app.yaml -n logging
```
Verify the running pods
```markdown
$ kubectl get pods -n logging
```
Verify the running service
```markdown
$ kubectl get service -n logging
```

Port Forward for Check Elastic Test Connection Form the current cli
```markdown
$ kubectl port-forward elasticsearch-0 9200:9200 -n logging & [1] 18200
```
And Check Kebana is access in browser
```bash
$ curl localhost:9200
```
Let’s get the count of the nodes.
```bash
$ curl -s localhost:9200/_nodes | jq ._nodes
```



### Step 5 — Creating the Kibana Deployment and Service on GKE
```markdown
$ kubectl create -f kibana-app.yaml -n logging
```
Verify the running pods
```markdown
$ kubectl get pods -n logging
```
Verify the running service
```markdown
$ kubectl get service -n logging
```
And Verify kibana 
```bash
$ kubectl port-forward kibana-87b7b8cdd-7wm5d 5601:5601 -n logging
```
And Check Kebana is access in browser
http://localhost:5601.

(Optional) Expose Elasticsearch service externally
```bash
$ kubectl patch svc "kibana" --namespace "logging" --patch '{"spec": {"type": "LoadBalancer"}}'
```

Get the Elasticsearch URL
```bash
$ SERVICE_IP=$(kubectl get svc "kibana" --namespace "logging"  --output jsonpath='{.status.loadBalancer.ingress[0].ip}')
```
And Than set Url
```bash
$  ELASTIC_URL="http://${SERVICE_IP}:5601"
```
And Check Kebana is access in browser
```bash
$ echo $ELASTIC_URL
```
And Check Kebana is access in browser
http://localhost:5601.


### Step 6 — Deploy Fluentd DaemonSet agent on GKE
- ServiceAccount:
- ClusterRole:
- ClusterRoleBinding:
- This grants the fluentd ServiceAccount the permissions listed in the 
```markdown
$ kubectl create -f elastic-fluentd.yaml -n logging
```
And Verify that your DaemonSet rolled out successfully using kubectl

````bash
$ kubectl get ds -n logging
````

### Step 7 — Visualize kubernetes data in Kibana

### Step 8 — (Optional) — Testing Container Logging



