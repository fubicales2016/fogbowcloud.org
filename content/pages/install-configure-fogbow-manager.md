Title: Install and configure fogbow manager
url: install-configure-fogbow-manager
save_as: install-configure-fogbow-manager.html
section: install-configure
index: 3

# Fogbow Manager

The Fogbow Manager is distributed in two forms: as source code or as a binary package for debian-based distributions. Choose the best distribution for your system, download it and install it as follow.

## Install from source
To get the lastest stable version of the component, download it from our repository:

```bash
wget https://github.com/fogbow/fogbow-manager/archive/master.zip
```

Then, decompress it:

```bash
unzip master.zip
```

Now, install it with Maven:

```bash
cd fogbow-manager
mvn install
```

## Install from debian package

Download a stable version from our <a href="http://downloads.fogbowcloud.org/stable/debian/">package repository</a> and install it with dpkg:

```bash
dpkg -i fogbow-manager_$version.deb
```

## Configure
After the installation, edit the file ```manager.conf```. In this file, some general properties, such as XMPP addresses and ports, as well as the set of plugins that define the behaviour of the Fogbow Manager are specified. Some of these properties need to be adjusted to match your particular cloud infrastructure provider. To ease the configuration procedure, we distribute template files for each cloud provider currenlty supported by the Fogbow middleware: ```manager.conf.cloudstack.example```,```manager.conf.opennebula.example```,```manager.conf.openstack.example``` and ```manager.conf.azure.example```. In this document, we cover in more details the general properties that might need to be changed and the main cloud-specific plugin properties. A complete overview of the Fogbow Manager plugins is given in the <a  href="/install-configure-plugins" target="_blank">Configure Fogbow Manager's Plugins </a> section of our documentation.

### General properties

The XMPP properties below indicate the XMMP server that the **Fogbow Manager** must contact to communicate in the federation. These properties also indicate the **Fogbow Rendezvous** associated with the **Fogbow Manager**.

```bash
# jid of the Fogbow Manager XMPP component
xmpp_jid=my-manager.internal.mydomain

# password of the Fogbow Manager XMPP component
xmpp_password=manager_password

# XMPP server IP address
xmpp_host=150.1.1.1

# Port in which the XMPP server will be listening.
xmpp_port=5347

# jid of your Rendezvous XMPP component
rendezvous_jid=my-rendezvous.internal.mydomain
```

The **xmpp_jid**, **xmpp_password** and **rendezvous_jid** properties above must match the values assigned during the <a  href="/install-configure-xmpp" target="_blank">Install and configure XMPP </a> section of our documentation.

Following, you need to add a new entry in your DNS to resolve the given **Fogbow Manager** component name to the IP address of the XMPP server like in the example below.

``` shell
my-manager.internal.mydomain        22      IN      A       150.1.1.1
```

**Time intervals:** The Fogbow Manager executes some background tasks, such as resource monitoring, periodically. All the time interval properties are defined in the class ```org.fogbowcloud.manager.core.ConfigurationConstants``` and have default period values specified in the class ```org.fogbowcloud.manager.core.ManagerController```. The default values of any property can be overwritten in the configuration file as shown below:

```bash
# The scheduler_period property is the time interval (in milliseconds) in which
# the Fogbow Manager submit requests that are not fulfilled yet
scheduler_period=30000

# The accounting_update_period is the time interval (in milliseconds) in which
# the Fogbow Manager calculate the resource usage accounting
accounting_update_period=300000
```

**SSH tunnel properties:** These properties configure the Fogbow Reverse Tunnel (FRT) service to provide public IP access to the VMs created by the Fogbow Manager in its intranet. The example below shows how these properties should be set, considering the example presented in the <a  href="/install-configure-frt" target="_blank">Install and configure Fogbow Reverse Tunnel </a> section of our documentation:

```bash
# The token_host_public_address property defines the public IP address of the FRT service
token_host_public_address=150.160.0.10

# The token_host_private_address property defines the private IP address of the FRT service
token_host_private_address=10.0.0.1

# The token_host_port property defines the port to be used when doing ssh.
# If this property isn't set, the default value is 2222
token_host_port=2222

# The token_host_http_port property defines the port to be used when communicating with the FRT
token_host_http_port=2223
```

**Manager HTTP port:** This property indicates the HTTP port to which the Fogbow Manager component endpoint will be listening to requests.

```bash
# The http_port property indicates http port of the Fogbow Manager service endpoint
http_port = 8182
```

### Cloud-specific plugins

#### OpenStack
```bash
## network Plugin
network_class=org.fogbowcloud.manager.core.plugins.network.openstack.OpenStackV2NetworkPlugin
network_openstack_v2_url=http://address:port
external_gateway_info=

## Storage Plugin
storage_class=org.fogbowcloud.manager.core.plugins.storage.openstack.OpenStackV2StoragePlugin
storage_v2_url=http://address:port

## Compute Plugin
compute_class=org.fogbowcloud.manager.core.plugins.compute.openstack.OpenStackNovaV2ComputePlugin
compute_novav2_url=http://address:port
compute_glancev2_url=http://address:port
compute_glancev2_image_visibility=private
compute_novav2_network_id=

## Compute Plugin (OCCI)
# compute_class=org.fogbowcloud.manager.core.plugins.compute.openstack.OpenStackOCCIComputePlugin
# compute_openstack_v2api_url=http://address:port
# compute_occi_url=http://address:port
# compute_occi_os_scheme=http://schemas.openstack.org/template/os#
# compute_occi_instance_scheme=http://schemas.openstack.org/compute/instance#
# compute_occi_resource_scheme=http://schemas.openstack.org/template/resource#
# compute_occi_network_id=
```
#### CloudStack
```bash
## Storage Plugin
# storage_class=org.fogbowcloud.manager.core.plugins.storage.cloudstack.CloudStackStoragePlugin

## Compute Plugin
compute_class=org.fogbowcloud.manager.core.plugins.compute.cloudstack.CloudStackComputePlugin
compute_cloudstack_api_url=http://address:port/client/api
compute_cloudstack_zone_id=
compute_cloudstack_image_download_base_url=http://address:port/downloads/
compute_cloudstack_image_download_base_path=/var/www/downloads/
compute_cloudstack_hypervisor=KVM
compute_cloudstack_image_download_os_type_id=
compute_cloudstack_expunge_on_destroy=true

## Local Identity
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.cloudstack.CloudStackIdentityPlugin
local_identity_url=http://address:port/client/api/

## Mapper Plugin / Local credentials
mapper_defaults_apiKey=user_api_key
mapper_defaults_secretKey=user_secret_key
```

#### Opennebula
```bash
# Network plugin
network_class=org.fogbowcloud.manager.core.plugins.network.opennebula.OpenNebulaNetworkPlugin
network_one_bridge=br0

## Storage Plugin
storage_class=org.fogbowcloud.manager.core.plugins.storage.opennebula.OpenNebulaStoragePlugin
## Default device prefix to use when attaching volumes, values: hd (IDE), sd (SCSI), vd (KVM), vxd (XEN)
storage_one_datastore_default_device_prefix=vd

## Compute Plugin
compute_class=org.fogbowcloud.manager.core.plugins.compute.opennebula.OpenNebulaComputePlugin compute_one_url=http://address:port/RPC2
compute_one_network_id=1 
compute_one_network_contextualization=false
compute_one_templates=all
compute_one_datastore_id=1
compute_one_ssh_host=address
compute_one_ssh_port=22
compute_one_ssh_username=
compute_one_ssh_key_file=/path/to/rsa/key
compute_one_ssh_target_temp_folder=/tmp/images

## Compute Plugin (Opennebula OCCI)
# compute_class=org.fogbowcloud.manager.core.plugins.compute.opennebula.OpenNebulaOCCIComputePlugin
# compute_one_url=http://address:port/RPC2
# compute_occi_url=http://address:port
# compute_occi_template_scheme=http://occi.localhost/occi/infrastructure/os_tpl#
# compute_occi_resource_scheme=http://schema.fedcloud.egi.eu/occi/infrastructure/resource_tpl#
# compute_occi_flavors_small={cpu=1,mem=512}
# compute_occi_flavors_medium={cpu=2,mem=1024}
# compute_occi_flavors_large={cpu=4,mem=2048}

## Local Identity
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.opennebula.OpenNebulaIdentityPlugin
local_identity_url=http://address:port/RPC2

# Mapper Plugin / Local credentials
mapper_defaults_username=
mapper_defaults_password=

## Federation Identity
federation_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin
federation_identity_url=http://address:port/RPC2

```
#### Azure

```bash
## Compute Plugin
compute_class=org.fogbowcloud.manager.core.plugins.compute.azure.AzureComputePlugin
compute_azure_max_vcpu=10
compute_azure_max_ram=10240
compute_azure_region=East US
compute_azure_storage_account_name=name
compute_azure_storage_key=key

## Identity
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.azure.AzureIdentityPlugin
mapper_defaults_subscription_id=subscription_id
mapper_defaults_keystore_path=/path/to/keystore
mapper_defaults_keystore_password=pass
```

## Run 
To start the manager component, run the start-manager script inside ```bin```.

```bash
./bin/start-manager
```
