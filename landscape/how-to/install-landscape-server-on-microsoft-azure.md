# How to install Landscape Server on Microsoft Azure

This guide provides an example of how to install and set up your Landscape server on Microsoft Azure with [cloud-init](https://cloudinit.readthedocs.io/en/latest/). The instructions here can be used for both FIPS-hardened or non-hardened systems.

> **For the most up-to-date documentation on Microsoft Azure, see [Microsoft Azure documentation](https://learn.microsoft.com/en-us/azure).

**Contents:**

- Install and set up Microsoft Azure CLI
- Provision Azure resources and deploy
- Deploy Landscape Server VM with cloud-init
- Configure Landscape
- (Optional) Perform a complete teardown


## Install and set up Microsoft Azure CLI

### Install `Azure CLI`

To install `Azure CLI`, refer to the [Install Azure CLI on Ubuntu](https://documentation.ubuntu.com/azure/en/latest/azure-how-to/instances/install-azure-cli/) guide.

### Connect `Azure` with your Microsoft Azure account

The Azure CLI's default authentication method for logins uses a web browser and access token to sign in.

Run the `az login` command:
```
az login
```

If the Azure CLI can open your default browser, it initiates authorisation code flow and opens the default browser to load an Azure sign-in page.

Sign in with your account credentials in the browser.


## Provision Azure resources and deploy

### Create a resource group

Create a resource group to contain all the Azure resources for deploying the VM.  The following command creates a resource group named `Landscape-rg` in the `eastus` location:
```
az group create --name Landscape-rg --location eastus
```

Output will be displayed in JSON format:
```
{
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/Landscape-rg",
  "location": "eastus",
  "managedBy": null,
  "name": "Landscape-rg",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```

### Create public-ip address resource

Create static IP address in the `Landscape-rg` resource group:
```
az network public-ip create \
	--resource-group Landscape-rg \
    --name LandscapePublicIP \
    --location eastus \
    --allocation-method Static
```

Output will be displayed in JSON format and will show static IP address (extract below):
```
"ipAddress": "34.139.255.120",
"ipTags": [],
"location": "eastus",
"name": "LandscapePublicIP",
"provisioningState": "Succeeded",
"publicIPAddressVersion": "IPv4",
"publicIPAllocationMethod": "Static",
"resourceGroup": "Landscape-rg",
```

Copy the IP address and set it as the A record value for the domain or subdomain that will serve as the FQDN. You set the A record in your DNS service. You can also use Azure DNS zone to host your DNS domain and manage your DNS records.

Verify the A record using `nslookup`. Replace `{landscape.domain.com}` with your FQDN:

```
nslookup {landscape.domain.com}
```

You’ll receive output similar to:
```
Server:		127.0.0.53
Address:	127.0.0.53#53
   
Non-authoritative answer:
Name:	landscape.domain.com
Address: 34.139.255.120
```

If the address value in the `nslookup` output matches the value of the `LandscapePublicIP` static IP address, the LetsEncrypt SSL provisioning step defined in the cloud-init configuration automation template will succeed.


## Deploy Landscape Server VM with cloud-init

Before beginning the deployment process with cloud-init, you must choose which of the two cloud-init configuration automation templates you want to use. In the [Landscape Scripts](https://github.com/canonical/landscape-scripts) Github repository, there are two Landscape Quickstart cloud-init configuration templates: [`cloud-init-quickstart.yaml`](https://github.com/canonical/landscape-scripts/blob/main/provisioning/cloud-init-quickstart.yaml) and [`cloud-init-quickstart-fips.yaml`](https://github.com/canonical/landscape-scripts/blob/main/provisioning/cloud-init-quickstart-fips.yaml).

The `cloud-init-quickstart.yaml` template is designed for anyone, and the `cloud-init-quickstart-fips.yaml` is designed for FIPS compliant deployments of Landscape Server. For more information, see [how to install FIPS hardened Landscape Server](https://ubuntu.com/landscape/docs/install-fips-hardened-landscape-server).

Once you’ve chosen your configuration template, complete the following steps.

1. Set the `IMAGE_FAMILY` environment variable based on the cloud-init configuration you chose.

- If you’re using `cloud-init-quickstart.yaml`, run:
```
curl -s https://raw.githubusercontent.com/canonical/landscape-scripts/main/provisioning/cloud-init-quickstart.yaml -o cloud-init.yaml
IMAGE_FAMILY=Canonical:0001-com-ubuntu-pro-jammy:pro-22_04-lts-gen2:latest
```

 - If you’re using `cloud-init-quickstart-fips.yaml`, run:
```
curl -s https://raw.githubusercontent.com/canonical/landscape-scripts/main/provisioning/cloud-init-quickstart-fips.yaml -o cloud-init.yaml
IMAGE_FAMILY=Canonical:0001-com-ubuntu-pro-focal-fips:pro-fips-20_04-gen2:latest
```

2. Open the downloaded cloud-init YAML file in an editor, determine which configuration parameters need to be changed between lines 4 and 32 and change these parameters.

The `HOSTNAME` on line 16 and `DOMAIN` on line 19 must be changed. Updating `EMAIL` on line 9, and adding your SendGrid API key on line 29 as the `SMTP_PASSWORD` are optional, but strongly recommended.


### Create VM

Run the following commands to create a VM and add a security rule to the network security group (NSG) to open port 80 and 443.  These ports are required to be open to allow the LetsEncrypt SSL provisioning step defined in the cloud-init to succeed.  The `--generate-ssh-keys` parameter causes the CLI to look for an available ssh key in `~/.ssh`. If one is found, that key is used. If not, one is generated and stored in `~/.ssh`.  The `--custom-data` parameter to pass in the cloud-init config file. Provide the full path to the `cloud-init.yaml` config if you saved the file outside of your present working directory:
```
az vm create \
    --resource-group Landscape-rg \
    --name LandscapeVM \
    --image $IMAGE_FAMILY \
    --size Standard_D2s_v3 \
    --admin-username azureuser \
    --assign-identity \
    --generate-ssh-keys \
    --public-ip-address LandscapePublicIP \
    --custom-data cloud-init.yaml
az vm open-port \
	--resource-group Landscape-rg \
	--name LandscapeVM \
	--port 80,443 \
	--priority 100
```

It takes a few minutes to create the VM and supporting resources.

> [!NOTE]
When creating the VM an error may occur with the code `MarketplacePurchaseEligibilityFailed`. This error indicates that before the subscription can use this image, you need to accept the legal terms of the image. Viewing and accepting the terms can be done via the Azure CLI. Refer to Microsoft Azure documentation [az vm image terms](https://learn.microsoft.com/en-us/cli/azure/vm/image/terms).

Observe the process by tailing the `cloud-init-output.log` file. Replace `{landscape.domain.com}` with your FQDN or static IP address:
```
ssh azureuser@{landscape.domain.com} 'tail -f /var/log/cloud-init-output.log'
```

A reboot may be required during the cloud-init process. If a reboot is required, you’ll receive the following output:
```
2023-08-20 17:30:04,721 - cc_package_update_upgrade_install.py[WARNING]: Rebooting after upgrade or install per /var/run/reboot-required
```

If the `IMAGE_FAMILY` specified earlier contained all the security patches, this reboot step may not occur.

Repeat the following code if a reboot was necessary to continue observing the log file:
```
ssh azureuser@{landscape.domain.com} 'tail -f /var/log/cloud-init-output.log'
```

Wait until the cloud-init process is complete. When it’s complete, you’ll receive the following line similar to this:
```
cloud-init v. 23.2.2-0ubuntu0~22.04.1 finished at Sun, 20 Aug 2023 17:30:56 +0000. Datasource DataSourceAzure [seed=/dev/sr0].  Up 37.35 seconds
```

Press `CTRL + C` to terminate the tail process in your terminal window.


## Configure Landscape

1. Navigate to the Landscape dashboard by entering the FQDN of the Landscape VM into a browser window

2. Provide a name, email address, and password for the first global administrator on the machine.
   
If the email address Landscape sends emails from should not be a subdomain based on the machine’s hostname, remove the hostname, or make the appropriate correction.

Alerts and administrator invitations sent via email are less likely to fail SPF or DMARC checks if the system email address is configured in a way the email service provider expects. If the email service provider sends emails which fail SPF and DMARC checks, mail delivery can be delayed or miscategorized as spam.
   

## (Optional) Perform a complete teardown

When no longer needed, you can delete the resource group to remove all the related resources used to create the Landscape Server VM.

To check the resources in the `Landscape-rg` resource group, run:
```
az resource list --resource-group Landscape-rg --output table
```

To delete the resource group, run:
```
az group delete --name Landscape-rg
```
