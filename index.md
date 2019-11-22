![Welcome Banner](https://github.com/GoogleCloudPlatform/click-to-deploy/blob/master/k8s/elastic-gke-logging/resources/elastic-gke-logging-architecture.png)

## How To Set Up an Elasticsearch, Fluentd and Kibana (EFK) Logging Stack on Kubernetes

We’ll begin by configuring and launching a scalable Elasticsearch cluster, and then create the Kibana Kubernetes Service and Deployment. To conclude, we’ll set up Fluentd as a DaemonSet so it runs on every Kubernetes worker node..

### Prerequisites

Before you begin with this guide, ensure you have the following available to you:

A Kubernetes 1.10+ cluster with role-based access control (RBAC) enabled

Ensure your cluster has enough resources available to roll out the EFK stack, and if not scale your cluster by adding worker nodes. We’ll be deploying a 3-Pod Elasticsearch cluster (you can scale this down to 1 if necessary), as well as a single Kibana Pod. Every worker node will also run a Fluentd Pod. The cluster in this guide consists of 3 worker nodes and a managed control plane.
The kubectl command-line tool installed on your local machine, configured to connect to your cluster. You can read more about installing kubectl in the official documentation.
Once you have these components set up, you’re ready to begin with this guide.

### Step 1

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/etechlearner/elastic-gke-logging/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
