# Azure Kubernetes Service (AKS) Deployment

## Create AKS cluster

1. In your jumpbox terminal, if you have not yet logged into your Azure subscription, then it's time to login.
    - If you are connected to the jumpbox via RDP then run
```az login```, which will open a browser and prompt you to login to Azure online.  After the browser says that you are logged in, close the browser to return to the terminal.
   - If you are connected to the jumpbox via SSH then run  
   ```az login -u <user ID> -p "<password>"```  
   E.g. ```az login -u odl-u12345 -p "AbC1234"```
   - If you are in Cloud Shell then you are already logged into Azure.
    
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
    RGNAME=<Resource Group name from output above>
    ```
1. Confirm the  variable is set correctly
    ```
    echo $RGNAME

    # Should output your Resource Group name, e.g. ODL-aks-v2-abc123
    ```

1. Name your AKS cluster. The name "<User ID>-aks" should be fine.  The name must be between 3 and 31 characaters in length, and begin with a letter. It can include letters, numbers and dashes. For example,
    ```
    CLUSTER_NAME=odl-uabc123-aks
    ```

1. Create your AKS cluster in the resource group created above with 2 nodes, targeting Kubernetes version 1.11.2
    ```
    # This command can take 5-25 minutes to run as it is creating the AKS cluster. Please be PATIENT...
    
    # set the location to one of the provided AKS locations (eg - centralus, eastus2, eastus)
    LOCATION=eastus2

    az aks create -n $CLUSTER_NAME -g $RGNAME -c 2 -k 1.11.2 --generate-ssh-keys -l $LOCATION --disable-rbac
    ```
    >NOTE: The "--disable-rbac" option is specified to simplify upcoming labs.  It is not recommended in a production environment.

1. If the cluster creation proces is still running after 20 minutes it is likely that something has gone awry.  In this case, to try and avoid any further delay, open another terminal on your jumpbox, and re-do the prior steps, setting the variables and executing the "az aks create" command, but this time **be sure to use a different value for CLUSTER_NAME**.

1. Verify your cluster status. The `ProvisioningState` should be `Succeeded`
    ```
    az aks list -o table

    Name                 Location    ResourceGroup         KubernetesVersion    ProvisioningState    Fqdn
    -------------------  ----------  --------------------  -------------------  -------------------  -------------------------------------------------------------------
    ODLaks-v2-gbb-16502  centralus   ODL_aks-v2-gbb-16502  1.11.2                Succeeded             odlaks-v2--odlaks-v2-gbb-16-b23acc-17863579.hcp.centralus.azmk8s.io
    ```


1. Get the Kubernetes config files for your new AKS cluster
    ```
    az aks get-credentials -n $CLUSTER_NAME -g $RGNAME
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
