# Azure Kubernetes Service (AKS) Deployment

## Create AKS cluster

1. Login to Azure Portal at http://portal.azure.com. Your Azure login ID will look something like `odl_u9294@csgtraining.onmicrosoft.com`
1. Open the Azure Cloud Shell

    ![Azure Cloud Shell](img/cloudshell.png "Azure Cloud Shell")
    >NOTE: You can safely do this lab in Cloud Shell, but you can laso do it in the jumpbox VM if you'd like.

1. Cloud Shell was already configured in Lab 0.  If you skipped that step, then please review Lab 0 and setup Cloud Shell.

1. Once your cloud shell is started, clone the workshop repo into the cloud shell environment
    ```
    git clone https://github.com/Azure/blackbelt-aks-hackfest.git
    ```

1. In the cloud shell, you are automatically logged into your Azure subscription. ```az login``` is not required.
    
1. Verify your subscription is correctly selected as the default
    ```
    az account list
    ```

1. Find your RG name

    ```
    az group list 
    ```
    
    ```
    [
    {
        "id": "/subscriptions/b23accae-e655-44e6-a08d-85fb5f1bb854/resourceGroups/ODL-aks-v2-gbb-8386",
        "location": "centralus",
        "managedBy": null,
        "name": "ODL-aks-v2-gbb-8386",
        "properties": {
        "provisioningState": "Succeeded"
        },
        "tags": {
        "AttendeeId": "8391",
        "LaunchId": "486",
        "LaunchType": "ODL",
        "TemplateId": "1153"
        }
    }
    ]
    ```

1. Copy the name from the results above and set to a variable 
    ```
    NAME=<Resource Group name from output above>
    ```
1. Confirm the NAME variable is set correctly
    ```
    echo $NAME

    # Should output your Resource Group name, e.g. ODL-aks-v2-gbb-8386
    ```

1. We might need to use a different cluster name, as sometimes the name in the group list has an underscore, and only dashes are permitted
    ```
    CLUSTER_NAME="${NAME//_}"
    ```

1. Create your AKS cluster in the resource group created above with 2 nodes, targeting Kubernetes version 1.11.2
    ```
    # This command can take 5-25 minutes to run as it is creating the AKS cluster. Please be PATIENT...
    
    # set the location to one of the provided AKS locations (eg - centralus, eastus)
    LOCATION=

    az aks create -n $CLUSTER_NAME -g $NAME -c 2 -k 1.11.2 --generate-ssh-keys -l $LOCATION --disable-rbac
    ```
    >NOTE: The "--disable-rbac" option is specified to simplify upcoming labs.  It is not recommended in a production environment.

1. Verify your cluster status. The `ProvisioningState` should be `Succeeded`
    ```
    az aks list -o table

    Name                 Location    ResourceGroup         KubernetesVersion    ProvisioningState    Fqdn
    -------------------  ----------  --------------------  -------------------  -------------------  -------------------------------------------------------------------
    ODLaks-v2-gbb-16502  centralus   ODL_aks-v2-gbb-16502  1.11.2                Succeeded             odlaks-v2--odlaks-v2-gbb-16-b23acc-17863579.hcp.centralus.azmk8s.io
    ```


1. Get the Kubernetes config files for your new AKS cluster
    ```
    az aks get-credentials -n $CLUSTER_NAME -g $NAME
    ```

1. Verify you have API access to your new AKS cluster

    > Note: It can take 5 minutes for your nodes to appear and be in READY state. You can run `watch kubectl get nodes` to monitor status. 
    
    ```
    kubectl get nodes
    
    NAME                       STATUS    ROLES     AGE       VERSION
    aks-nodepool1-20004257-0   Ready     agent     4m        v1.11.2
    aks-nodepool1-20004257-1   Ready     agent     4m        v1.11.2
    ```
    
    To see more details about your cluster: 
    
    ```
    kubectl cluster-info
    
    Kubernetes master is running at https://odlaks-v2--odlaks-v2-gbb-11-b23acc-115da6a3.hcp.centralus.azmk8s.io:443
    Heapster is running at https://odlaks-v2--odlaks-v2-gbb-11-b23acc-115da6a3.hcp.centralus.azmk8s.io:443/api/v1/namespaces/kube-system/services/heapster/proxy
    KubeDNS is running at https://odlaks-v2--odlaks-v2-gbb-11-b23acc-115da6a3.hcp.centralus.azmk8s.io:443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
    kubernetes-dashboard is running at https://odlaks-v2--odlaks-v2-gbb-11-b23acc-115da6a3.hcp.centralus.azmk8s.io:443/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy

    To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
    ```

You should now have a Kubernetes cluster running with 2 nodes. You do not see the master servers for the cluster because these are managed by Microsoft. The Control Plane services which manage the Kubernetes cluster such as scheduling, API access, configuration data store and object controllers are all provided as services to the nodes. 
