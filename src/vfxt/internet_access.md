
# Internet access for Avere vFXT clusters

The following shows how to handle restricted internet access, by configuring any of Internet access, DNS, and proxy:

## Scenario: Internet Blocked

Here are the requirements for a locked down network security group:
  * Open up `download.averesystems.com`, TCP port `443`
  * ensure "Microsoft.Service" endpoint is setup on all Virtual Network Subnets
  * Set NTP in the Avere settings to 169.254.169.254
  * Open up access to TCP port `443` to 'Storage.REGION' as shown in the below image:

  ![Network Security group outbound rules showing opening up TCP port `443` to 'AzureConnectors' and 'AzureCloud'](../../docs/images/outboundrules.png)

If you have a proxy, see the proxy section below.

## Required Ports on an NSG

The following are the required ports to be open for the Avere vFXT: https://azure.github.io/Avere/legacy/ops_guide/4_7/html/required_ports.html.

## Scenario: Bring your own DNS Server

Here are the requirements for a "Bring your own DNS Server" scenario:
  * Add `management.azure.com` to DNS Server
  * Add `download.averesystems.com` to DNS Server
  * DNS forward the Azure Storage account dns name to a Microsoft DNS server.  Note that storage accounts change IP addresses frequently, so adding a static entry will eventually fail.
 
## Scenario: Proxy (Advanced)

This advanced scenario can be done via deployment of the template `azuredeploy-auto.json`.  You can configure a proxy by adjusting the `additionalVFXTParameters`:
  * `--proxy-uri http://PROXY_IP:PROXY_PORT`
  * `--cluster-proxy-uri http://PROXY_IP:PROXY_PORT`

For example you would adjust the `additionalVFXTParameters` variable in the template `azuredeploy-auto.json` to the following:

```json
"additionalVFXTParameters": "[concat(' --nodes ', variables('avereNodeCount'), if(variables('enableCloudTraceDebugging'),' --skip-cleanup ',''), '--proxy-uri http://PROXY_IP:PROXY_PORT --cluster-proxy-uri http://PROXY_IP:PROXY_PORT', ' --debug')]",
```


