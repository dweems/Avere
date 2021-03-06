# NFS NAS Core Filer for LSv1

The templates in this folder implements an NFS based NAS Filer using the LSv1 series SKU on Azure as described on the LS-Series page: https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-previous-gen#ls-series.

The pre-requisites for this template are the following:
1. an SSH public key,
2. a VNET already created

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmain%2Fsrc%2Ftutorials%2Fnfsfiler-LSv1%2Fnfs-azuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

The template can also be deployed through the az cli using the following steps:

1. open https://shell.azure.com

2. set the correct subscription, replacing your azure subscription id with `AZURE_SUBSCRIPTION_ID`:

```bash
az account set --subscription AZURE_SUBSCRIPTION_ID
```

3. download the templates

```bash
curl -o nfs-azuredeploy.parameters.json https://raw.githubusercontent.com/Azure/Avere/main/src/tutorials/nfsfiler-LSv1/nfs-azuredeploy.parameters.json
curl -o nfs-azuredeploy.json https://raw.githubusercontent.com/Azure/Avere/main/src/tutorials/nfsfiler-LSv1/nfs-azuredeploy.json
```
4. edit file `nfs-azuredeploy.parameters.json`, and set the correct values

5. deploy the template, and capture the output variables to get the IP address, and exported path.

```bash
export DstResourceGroupName="nfsfiler1"
export DstLocation="uksouth"
az group create --name $DstResourceGroupName --location $DstLocation
az group deployment create --resource-group $DstResourceGroupName --template-file nfs-azuredeploy.json --parameters @nfs-azuredeploy.parameters.json
```

# Design Considerations

* custom script extension is used instead of cloud-init (customData) to get feedback on success or failure of the script
* for simplicity the install script is downloaded from Github, but you could upload the script to a locked down blob storage account, and specify the script in the variable `installScriptBlobUrl` 
* retry logic throughout script to handle cloud failures
* nfs
  * the nfs server uses 64 threads, for maximum performance
  * sync is specified for data durability in case the nfs server crashes
  * no_root_squash is used on the nfs filer so it is ready for HPC Cache or and Avere vFXT to mount it (via NFS v3)