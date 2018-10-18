# Kubernetes Dashboard

The Kubernetes dashboard is a web ui that lets you view, monitor, and troubleshoot Kubernetes resources. 

> Note: The Kubernetes dashboard is a secured endpoint and can only be accessed using the SSH keys for the cluster. You can only complete this lab if you can RDP into your jumpbox.  If not, simply skip this lab.

### Accessing The Dashboard UI

There are multiple ways of accessing Kubernetes dashboard. You can access it through the `kubectl` or `az` command-line interface or directly through the master server API. We'll be using ```az aks browse``` as it simplifies the process of creating a secure local proxied connection to the API Server and doesn't expose the UI to the internet.

1. Command-Line using the Azure CLI

    * Open an RDP session to the jumpbox IP with username and password
    * If you're not already logged into Azure via the CLI, run ```az login -u <User ID> -p "<password>"``` to authenticate with Azure
    * Run ```RGNAME=<your Resource Group name>```
    * Run ```CLUSTER_NAME=<your AKS cluster name>```
    * Run ```az aks browse -n $CLUSTER_NAME -g $RGNAME``` to open the Kubernetes Dashboard UI in a web browser (Firefox is pre-installed on the Jumpbox)

### Explore Kubernetes Dashboard

1. In the Kubernetes Dashboard select nodes to view
![](img/ui_nodes.png)
2. Explore the different node properties available through the dashboard
3. Explore the different pod properties available through the dashboard ![](img/ui_pods.png)
4. In this lab feel free to take a look around other at  other resources Kubernetes provides through the dashboard

> To learn more about Kubernetes objects and resources, browse the documentation: <https://kubernetes.io/docs/user-journeys/users/application-developer/foundational/#section-3>
