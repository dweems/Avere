# Virtual Machine Client Implementations that mount the Avere vFXT Edge Filer

This folder includes three virtual machine (VM) implementations to deploy multiple VMs mounted to the Avere vFXT: loose VMs, VM availability sets (VMAS), and VM scale sets (VMSS).

The clients are mounted roundrobin across the Avere vFXT vServer IP addresses done by the bootstrap script stored on the vFXT.  Before deploying the clients, you must install the bootstrap script to the Avere vFXT.  To do this, on the cluster controller, mount the vFXT shares. 

1. Run the following commands:

```bash
sudo -s
apt-get update
apt-get install nfs-common
mkdir -p /nfs/node1
mkdir -p /nfs/node2
mkdir -p /nfs/node3
chown nobody:nogroup /nfs/node1
chown nobody:nogroup /nfs/node2
chown nobody:nogroup /nfs/node3
```

2. Edit `/etc/fstab` to add the following lines but *using your vFXT node IP addresses*. Add more lines if your cluster has more than three nodes.

```bash
10.0.0.12:/msazure	/nfs/node1	nfs hard,nointr,proto=tcp,mountproto=tcp,retry=30 0 0
10.0.0.13:/msazure	/nfs/node2	nfs hard,nointr,proto=tcp,mountproto=tcp,retry=30 0 0
10.0.0.14:/msazure	/nfs/node3	nfs hard,nointr,proto=tcp,mountproto=tcp,retry=30 0 0
```

3. To mount all shares, type `mount -a` from the cluster controller, and then run the following to download the bootstrap script.

```bash
mkdir -p /nfs/node1/bootstrap
cd /nfs/node1/bootstrap
curl --retry 5 --retry-delay 5 -o /nfs/node1/bootstrap/bootstrap.sh https://raw.githubusercontent.com/Azure/Avere/master/src/clients/bootstrap.sh
```

# Loose Virtual Machines

To understand how to maximize boot speed of these VMs, please review [Best Practices for Improving Azure Virtual Machine (VM) Boot Time](../../docs/azure_vm_provision_best_practices.md).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fclients%2Fvmas%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

# Virtual Machine Availability Sets (VMAS)

To understand how to maximize boot speed of these VMs, please review [Best Practices for Improving Azure Virtual Machine (VM) Boot Time](../../docs/azure_vm_provision_best_practices.md).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fclients%2Fvmas%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

# Virtual Machine Scale Sets (VMSS)

To understand how to maximize boot speed of these VMs, please review [Best Practices for Improving Azure Virtual Machine (VM) Boot Time](../../docs/azure_vm_provision_best_practices.md).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fclients%2Fvmss%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>