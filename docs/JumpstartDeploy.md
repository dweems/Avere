# Jumpstart Deploy a vFXT Cluster
The easiest way to create a vFXT cluster, is to use a controller node which has scripts and templates for creating the vFXT cluster. In this tutorial, you will create a controller node from the Azure portal and use it to create a vFXT cluster.

[Click here](JumpstartDeployPrint.md) for a printer-friendly version of this tutorial.

This Jumpstart tutorial assumes that you are a subscription owner and that you have enough quota to run vFXT instances. Consider creating a new subscription to track project expenses.

## Create Controller

### Find the Avere controller node image
From the [Azure marketplace](https://ms.portal.azure.com/#blade/Microsoft_Azure_Marketplace/GalleryFeaturedMenuItemBlade/selectedMenuItemId/home), search for Avere and click the Controller node.

<img src="images/1selectcontroller.png">

### Enable Programmatic Access
The controller node needs programmatic access. You’ll need to enable programmatic access to your subscription BELOW the Create button. You’ll only need to do this once and can skip this step in the future. 

<img src="images/2 - programmatic access link b.png">

<img src="images/3 - enable programmatic access b.png">

After enabling programmatic access for the controller node, select the vFXT node and enable programmatic access for that image.

### Walk through the wizard
Click Create to walk through the wizard.
Name the instance.
Choose HDD.
Provide a username and password for command-line access. 
Choose your subscription.
Create a new resource group.
Choose a location close to you.
Before clicking OK, note the username, password, resource group name, and location.

<img src="images/4 - capture page 1 b.png">

For the size, choose A0.

Create a new virtual network and subnet. Capture this information for later.
To access the controller remotely, we’ll need a public IP address and SSH.

<img src="images/5 - vnet info b.png">

We don’t need boot diagnostics but we do need to register with Azure AD.

<img src="images/6 - boot and MSI b.png">

Review and Create. After 5 or 6 minutes, your controller node will be up and running.

## Create cluster
Now that your controller node is running, you need to access the controller node, edit the templates, and run the create cluster script. 

### Access the Controller
Copy your controller’s public IP address from the portal.

<img src="images/7publicip.png">

SSH to the device with the username and password that you provided.
The following steps are mentioned in the `/VFXT_README` file.

<img src="images/8sshreadme.png">

Authenticate by running `az login`.

<img src="images/9azlogin.png">

Copy your subscription ID.

<img src="images/10subid.png">

Run ```az account set --subscription``` and paste your subscription ID.

<img src="images/11setsub.png">

### Edit the templates
Edit the cluster role template (`vi /avere-cluster.json`) and paste your subscription ID here, too. This role gives the controller permissions to create the vFXT and the Azure components it needs.

<img src="images/12pastesubid.png">

Create the role by running the az role definition create command `az role definition create --role-definition /avere-cluster.json`

Copy and then edit the `create-minimal-cluster` template. For example:
```cp /create-minimal-cluster ./cmc```
```vi cmc```

Provide the name of your resource group, location, virtual network, subnet, and cluster role. Give your cluster a name and an admin password.

<img src="images/13edittemplate.png">

Save the file and exit.

### Run the script
Run the script (`./cmc`). When the script completes, copy the management IP address.

<img src="images/14mgmtip.png">