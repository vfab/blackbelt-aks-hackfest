# Lab Environment

## Classroom Setting

These labs are designed for delivery in a classroom setting with the **Azure Global Blackbelt Team and Cardinal Solutions.** We typically provide an Azure subscription and a Linux VM (jumpbox) for attendees to complete the labs.

### Getting Credentialed

* Get your Azure credentials from your hackfest instructor.

### Setup Environment

* The labs will require you to RDP (or SSH) into a Linux jumpbox in the Azure subscription created for you.
    * Ensure you have a proper RDP client on your PC.
    * On the Mac, use Remote Desktop Client in the App Store.
    * If you cannot RDP into the jumbox, then you can SSH in.  Just be aware that certain lab steps will only work when using RDP, but you can still complete the labs.
* As a fall back option, setup Azure Cloud Shell: 

    1. Browse to http://portal.azure.com
    2. Login with the Azure credentials you were provided in the prior steps.  To login you must use the email corresponding to your user ID, which will include the Azure tenant domain, which is "csgtraining.onmicrosoft.com". So your lab email will look something like "odl_u12345@csgtraining.onmicrosoft.com".
    3. Either click on the cloud shell icon to start your session.

        ![alt text](img/cloud-shell-start.png "Spektra ready")
        ...OR for a full page experience, open a new browser tab and go to http://shell.azure.com.

    4. Select `Bash (Linux)`
    5. You will be prompted to setup storage for your cloud shell. Click `Show advanced settings`

        ![alt text](img/cloud-show-advanced.png "Spektra ready")

    6. Provide a unique value for Storage account name. This must be all lower case and no punctuation. Use "cloudshell" for File share name. See example below.

        ![alt text](img/cloud-storage-config.png "Spektra ready")

    7. Click `Create storage`


## Self-guided

It is possible to use your own machine outside of the classroom. You will need the following in order to complete these labs: 

* Azure subscription
* Linux, Mac, or Windows with Bash
* Docker
* Azure CLI
* Visual Studio Code
* Helm
* Kubernetes CLI (kubectl)
* MongoDB (only lab #1 requires this)
* GitHub account and git tools
