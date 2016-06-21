Title: Customising and deployment
url: customazing-deployment
save_as: customazing-deployment.html
section: customazing-deployment
index: 1

# Plugins

The manager component was designed to be agnostic to the underlying cloud technology. There is an interoperability plugin layer between this component and the cloud, in a sense that plugins are responsible for translating fogbow's orders to what the underlying cloud understands. Plugins are instantiated via reflection, and fully configured via the configuration file. Currently, fogbow makes available some plugins, and new ones can be contributed by the fogbow developers' community.

In addition to interoperability plugins, there are also behavioural plugins. These are used to specify the way each fogbow manager should act when serving client's orders.

## Identity Plugin

The Identity Plugin is responsible for getting and authenticating tokens for local and federation cloud users. The Local Identity Provider and the Federation Identity Provider must be the same when the authentication is done at the Local Cloud Identity Provider. Otherwise, you can configure different plugins for local and federation identity providers. 

Make sure to have the identity plugin ports opened in your network firewall configuration, to allow authentication by federation users from outside the local network, If you are using Keystone Identity Plugin, open the port 5000, or the port 2633 for OpenNebula Identity Plugin.

### Configure

As you can see at the [Manager Install Guide](http://www.fogbowcloud.org/install-manager), when the installation is done, rename the file ```manager.conf.example``` to ```manager.conf```. You need to configure the Identity Plugin according to the Identity Provider you are using. The following sections go through the configuration for the Identity Plugins that come along with the Fogbow Manager. You can configure different plugins for local and federation identity providers.

#####Keystone Identity Plugin

```bash

# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.openstack.KeystoneIdentityPlugin
# Cloud Identity endpoint
local_identity_url=http://localhost:5000

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.openstack.KeystoneIdentityPlugin
# Federation Identity endpoint
federation_identity_url=http://localhost:5000

# Proxy account for remote requests @ the local identity provider
local_proxy_account_user_name=fogbow
# Password of such account
local_proxy_account_password=fogbow
# Tenant of such account
local_proxy_account_tenant_name=demo
```

#####OpenNebula Identity Plugin

```bash

# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin
# Cloud identity endpoint
local_identity_url=http://localhost:2633/RPC2

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin
# Federation Identity endpoint
federation_identity_url=http://localhost:2633/RPC2

# Proxy account for remote order @ the local identity provider 
local_proxy_account_user_name=fogbow
# Password of such account
local_proxy_account_password=fogbow

```

#####X509 Plugin

```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.x509.X509IdentityPlugin

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.x509.X509IdentityPlugin

# Directory where are the certificates of Certificate Authorities (CA). 
# They are certificates that you trust.
x509_ca_dir_path=/path/to/ca/directory
```

#####VOMS Plugin

```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.voms.VomsIdentityPlugin

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.voms.VomsIdentityPlugin

# Directory where are the VOMS server information. 
# List of voms servers in order to issue a proxy. 
# Default : "~/.glite/vomses"
path_vomses=/path/vomes

# Directory where are the certificates of Certificate Authorities (CA). 
# They are certificates that you trust.
# You need to have the certificate, CRL (certificate revocation list), 
# info, namespaces and signing_policy files for each CA.
# These files need to have read permission grant to the user that runs
# the fogbow manager
# Default : "/etc/grid-security/certificates"
path_trust_anchors=/path/trust/anchors

# Directory where are the certificates of VOMS that you trust.
# Default : "/etc/grid-security/vomsdir"
path_vomsdir=/path/voms/dir

```
Note: The VOMS Plugin uses the <a href="https://github.com/italiangrid/voms-api-java" target=_blank>VOMS API Java</a>. This API only works with JREs provided by Oracle with the <a href="http://stackoverflow.com/questions/6481627/java-security-illegal-key-size-or-default-parameters" target=_blank>unlimited strength file installed</a>.

##### No Cloud Identity Plugin
Cloud Compute Plugin describe a scenary that does not exist an cloud that is associate to a Fogbow manager.
```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.nocloud.NoCloudIdentityPlugin
```

##### Simple Token Identity Plugin
```bash
# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.simpletoken.SimpleTokenIdentityPlugin
# Token to check
simple_token_identity_valid_token_id=9398ybc43r-c9871btr7
```

##### EC2 Identity Plugin
```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.ec2.EC2IdentityPlugin

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.ec2.EC2IdentityPlugin
```
##### Azure Identity Plugin
```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.azure.AzureIdentityPlugin

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.azure.AzureIdentityPlugin
```
##### CloudStack Identity Plugin
```bash
# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.cloudstack.CloudStackIdentityPlugin
# Federation Identity endpoint
federation_identity_url=http://127.0.0.1:8080/client/api/

# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.cloudstack.CloudStackIdentityPlugin
# Cloud Identity endpoint
local_identity_url=http://127.0.0.1:8080/client/api/
```
##### Shiboleth Identity Plugin
```bash
# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.identity.shibboleth.ShibbolethIdentityPlugin
```

## Compute Plugin

The Compute Plugin is responsible for requesting, getting, and deleting instances at the local cloud. Different plugins can require different information depending on their implementation. Fogbow manager assumes that all cloud users have quota defined and all information at ```manager.conf``` file are correct. If not, the behaviour of federation may not be the expected.

### Configure

As you can see at the [Manager Install Guide](http://www.fogbowcloud.org/install-manager), after installation move the file ```manager.conf.example``` to ```manager.conf```. You need to add the compute plugin contents according to plugin that will be used. These examples show the required properties for plugins already available on fogbowcloud project. 

##### OpenStack OCCI Compute Plugin

```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackComputePlugin

# Cloud OCCI endpoint
compute_occi_url=http://localhost:8182

# Cloud v2 compute endpoint
compute_openstack_v2api_url=http://localhost:8182

# Associating local cloud flavors to fogbow flavors
# Small flavor
compute_occi_flavor_small=m1-small

# Medium flavor
compute_occi_flavor_medium=m1-medium

# Large flavor
compute_occi_flavor_large=m1-large

# OS Cloud Scheme
compute_occi_os_scheme=http://schemas.openstack.org/template/os#

# Instance Cloud Scheme
compute_occi_instance_scheme=http://schemas.openstack.org/compute/instance#

# Resource Cloud Scheme
compute_occi_resource_scheme=http://schemas.openstack.org/template/resource#

# Network ID (This property is required only if user project has more 
# than one network available)
# Example:
compute_occi_network_id=ea51ed0c-0e8a-448d-8202-c79777109ffe
```

##### OpenStack Nova V2 Compute Plugin
```bash
# Plugin class
compute_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackNovaV2ComputePlugin

# Nova V2 API URL
compute_novav2_url=http://localhost:8774

# Small Flavour Identifier
compute_novav2_flavor_small=1

# Medium Flavour Identifier
compute_novav2_flavor_medium=2

# Large Flavour Identifier
compute_novav2_flavor_large=3

# Network id (This property is required only if user project has more 
# than one network available)
compute_novav2_network_id=ea51ed0c-0e8a-448d-8202-c79777109ffe
```

##### OpenNebula Compute Plugin

```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaComputePlugin

# Cloud opennebula compute endpoint
compute_one_url=http://localhost:2633/RPC2

# Associating properties flavors to fogbow flavors
# Small flavor
compute_one_flavor_small={mem=128, cpu=1}

# Medium flavor
compute_one_flavor_medium={mem=256, cpu=2}

# Large flavor
compute_one_flavor_large={mem=512, cpu=4}

# Network ID to be used for instances
compute_one_network_id=1

# Settings used by ONE compute plugin to register new images in the cloud
# ID of datastore to register the image
compute_one_datastore_id=1
# To register a new image, the image file needs to be in the some machine where ONE is running
# If the fogbow manager is running in a different machine, set the SSH properties to transfer the image
# Or if the fogbow manager is in the same machine, leave it blank
compute_one_ssh_host=127.0.0.1
compute_one_ssh_port=22
compute_one_ssh_username=fogbow
# The SSH try to access using private key, set the path to ssh id_rsa file
compute_one_ssh_key_file=/home/fogbow/.ssh/id_rsa
# Set the directory to copy images in remote host
compute_one_ssh_target_temp_folder=/tmp/images
```
##### No Cloud Compute Plugin
Cloud Compute Plugin describe a scenary that does not exist an cloud  that is associate to a Fogbow manager.
```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.compute.nocloud.NoCloudComputePlugin
```
##### EC2 Compute Plugin
```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.compute.ec2.EC2ComputePlugin
# Region where will create the VM
compute_ec2_region=us-east-1
# Security group id given in the user's account
compute_ec2_security_group_id=sg-12345678
# Subnet id given in the user's account
compute_ec2_subnet_id=subnet-12345678
# 
compute_ec2_image_bucket_name=s3-bucket-for-images
# amount maximum of vCPU
compute_ec2_max_vcpu=10
# amount maximum of RAM
compute_ec2_max_ram=10240
# amount maximum of instances
compute_ec2_max_instances=10
```

##### Azure Compute Plugin
```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.compute.azure.AzureComputePlugin
compute_azure_max_vcpu=10
compute_azure_max_ram=10240
compute_azure_region=East US
compute_azure_storage_account_name=storage1
compute_azure_storage_key=abcd12345
``` 

##### CloudStack Compute Plugin
```bash
# Compute plugin class
compute_class=org.fogbowcloud.manager.core.plugins.compute.cloudstack.CloudStackComputePlugin
compute_cloudstack_api_url=http://127.0.0.1:8080/client/api
compute_cloudstack_zone_id=d05bfc3e-85e5-4be8-9ae9-cc7c2deb95f1
compute_cloudstack_image_download_base_url=http://127.0.0.1/downloads/
compute_cloudstack_image_download_base_path=/var/www/downloads/
compute_cloudstack_hypervisor=KVM
compute_cloudstack_image_download_os_type_id=ea51ed0c-0e8a-448d-8202-c79777109ffe
compute_cloudstack_expunge_on_destroy=true
``` 

## Image Storage Plugin

New orders arrive at the Fogbow Manager describe, among other things, an image that should be used by instances spawned. This image is a federation-wide id and potentially not recognized at the underlying cloud level as a proper image identifier. Image Storage plugins do the job of parsing such ids and translating them to valid local image identifiers.

##### VMCatcher Storage Plugin

Allows users to subscribe to virtual machine Virtual Machine Image Lists following the HEPIX Virtualisation working groups specification, cache the images referenced to in the Virtual Machine Image List, validate the images list with x509 based public key cryptography, and validate the images against sha512 hashes in the images lists and provide events for further applications to process updates or expiries of virtual machine images without having to further validate the images.

For more information about vmcatcher, access [vmcatcher page on github.](https://github.com/hepix-virtualisation/vmcatcher)
```bash
## Image Storage Plugin (VMCatcher)
# Image storage class
image_storage_class=org.fogbowcloud.manager.core.plugins.imagestorage.vmcatcher.VMCatcherStoragePlugin
# Run with "sudo"
image_storage_vmcatcher_use_sudo=false
# Path database
image_storage_vmcatcher_env_VMCATCHER_RDBMS="sqlite:////var/lib/vmcatcher/vmcatcher.db"
# Path where stores the images downloaded
image_storage_vmcatcher_env_VMCATCHER_CACHE_DIR_CACHE="/var/lib/vmcatcher/cache"
# Path where stores the images are being download
image_storage_vmcatcher_env_VMCATCHER_CACHE_DIR_DOWNLOAD="/var/lib/vmcatcher/cache/partial"
# Path where stores the images expired
image_storage_vmcatcher_env_VMCATCHER_CACHE_DIR_EXPIRE="/var/lib/vmcatcher/cache/expired"

## Use "glancepush specification" for openstack or "one specification" for opennebula

### Plugin for openstack
### glancepush specific
## Option for use the openstack plugin
# image_storage_vmcatcher_push_method=glancepush
# image_storage_vmcatcher_glancepush_vmcmapping_file=/etc/vmcatcher/vmcmapping
## Path of the plugin
# image_storage_vmcatcher_env_VMCATCHER_CACHE_EVENT="python /var/lib/vmcatcher/gpvcmupdate.py"

### Plugin for opennebula
### one specific
## Option for use the openstack plugin
# image_storage_vmcatcher_push_method=cesga
## Path of the plugin
# image_storage_vmcatcher_env_VMCATCHER_CACHE_EVENT="python /var/lib/vmcatcher/vmcatcher_eventHndl_ON"
## Path where are the user's credentiais
# image_storage_vmcatcher_env_ONE_AUTH="/etc/vmcatcher/one_auth"
```

##### HTTP Download Image Storage Plugin
This plugin allows users to request instances using an image URL, that should be an absolute URL or a suffix that will be appended to the base URL configured, then the plugin tries to download and register the new image in the cloud if it doesn't exists.

```bash
image_storage_class=org.fogbowcloud.manager.core.plugins.imagestorage.http.HTTPDownloadImageStoragePlugin

# Base URL of the image repository like the EGI Applications Database
# If the user supplies an relative URL in the request, the base URL will be used to find the image
image_storage_http_base_url=http://appliance-repo.egi.eu/images

# Path to the temporary directory where the images should be downloaded to
image_storage_http_tmp_storage=/tmp/
```

##### Static Image Storage Plugin
```bash
image_storage_static_fogbow-linux-x86=
image_storage_static_fogbow-ubuntu-12.04-with-java=
```

As you can see above, you can statically configure the fogbow manager with as many image as you want. Therefore, each manager can be configured with different images and/or same images but different names. Currently, fogbow uses global image identifiers in requests. For example, if the user requests for one instance of **image-ubuntu** in cloud A and that cloud does not have such image, the request is passed on to another cloud, cloud B, and will be fulfilled only if the manager in cloud B was configured with **image-ubuntu** or it previously fetched such image from a VM marketplace. A list of the most commom image names used by current fogbow installations follows on:

 * **fogbow-linux-x86**: Cirros 0.3.3 image; cirros as username; and cubswin:) as password
 * **fogbow-ubuntu-12.04-with-java**: Ubuntu 12.04 image (standard ubuntu cloud image with java); ubuntu as username; SSH login via publickey.

Furthermore, Fogbow also expects that all images configured at manager have **cloud-init** working properly.

## Authorization Plugin

The Authorization Plugin tells whether a given user (with a proper authenticated token) is able to create requests in the Federation. 

### Configure

The **federation_authorization_class** property must be set to a Authorization Plugin implementation. The Fogbow Manager comes with a single implementation that simply authorizes any user/token.

##### Allow All Authorization Plugin

```bash
# Federation Authorization plugin class
# Example 
federation_authorization_class=org.fogbowcloud.manager.core.plugins.common.AllowAllAuthorizationPlugin
```
##### VO White List Authorization Plugin
```bash
federation_authorization_class=org.fogbowcloud.manager.core.plugins.authorization.voms.VOWhiteListAuthorizationPlugin
authorization_vo_whitelist=memberOfListOne, memberOfListTwo, memberOfListThree
```
##### Edu Person White List Authorization Plugin
```bash
federation_authorization_class=org.fogbowcloud.manager.core.plugins.authorization.eduperson.EduPersonWhitelistAuthorizationPlugin
authorization_vo_whitelist=memberOfListOne, memberOfListTwo, memberOfListThree
```

## Storage Plugin
The Storage Plugin is responsible for requesting, getting, and deleting storage at the local cloud. Different plugins can require different information depending on their implementation. Fogbow manager assumes that all cloud users have quota defined and all information at ```manager.conf``` file are correct. If not, the behaviour of federation may not be the expected.

### Configure

##### OpenStack V2 Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.openstack.OpenStackV2StoragePlugin
storage_v2_url=http://localhost:8776
```
##### Opennebula Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.opennebula.OpenNebulaStoragePlugin
# Default device prefix to use when attaching volumes, values: hd (IDE), sd (SCSI), vd (KVM), vxd (XEN)
storage_one_datastore_default_device_prefix=vd
```
##### No Cloud Storage Plugin
Cloud Storage Plugin describe a scenary that does not exist an cloud  that is associate to a Fogbow manager.
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.cloudstack.CloudStackStoragePlugin
```
##### EC2 Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.ec2.EC2StoragePlugin
storage_ec2_availability_zone=us-east-1b
```
##### Azure Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.azure.AzureStoragePlugin
compute_azure_storage_account_name=
compute_azure_storage_key=
```
##### CloudStack Storage Plugin
```bash
# Storage Plugin class
storage_class=org.fogbowcloud.manager.core.plugins.storage.cloudstack.CloudStackStoragePlugin
```

## Network Plugin
The Network Plugin is responsible for requesting, getting, and deleting storage at the local cloud. Different plugins can require different information depending on their implementation. Fogbow manager assumes that all cloud users have quota defined and all information at ```manager.conf``` file are correct. If not, the behaviour of federation may not be the expected.

### Configure
##### OpenStack V2 Network Plugin
```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.openstack.OpenStackV2NetworkPlugin
network_openstack_v2_url=http://localhost:9696
external_gateway_info=ea51ed0c-0e8a-448d-8202-c79777109ffe
```
##### Opennebula Network Plugin
```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.opennebula.OpenNebulaNetworkPlugin
network_one_bridge=br0
```
##### No Cloud Network Plugin
Cloud Compute Plugin describe a scenary that does not exist an cloud  that is associate to a Fogbow manager.

```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.nocloud.NoCloudNetworkPlugin
```
##### EC2 Cloud Network Plugin
```bash
# Network Plugin class
network_class=org.fogbowcloud.manager.core.plugins.network.ec2.EC2NetworkPlugin
network_ec2_region=
```

## Accounting Plugin
The Accounting Plugin is responsible for accounting of the instances and storages. 
### Configure
Configuração comum: 
```bash
# Periodo de atualização do accouting em milisegundos
accounting_update_period=300000
```

##### FCU Accounting Plugin
O calculo é feito pela soma de minutos usados mais a potência determinada pelo plugin do benchmarking.

```bash
# Accounting class
accounting_class=org.fogbowcloud.manager.core.plugins.accounting.FCUAccountingPlugin
# Path database
fcu_accounting_datastore_url=jdbc:sqlite:/tmp/computeusage
```

##### Simple Storage Accounting Plugin
O calculo é feito pela soma de minutos usados mais o tamanho do storage.

```bash
# Accounting class
storage_accounting_class=org.fogbowcloud.manager.core.plugins.accounting.SimpleStorageAccountingPlugin
# Path database
simple_storage_accounting_datastore_url=jdbc:sqlite:/tmp/storageusage
```

## Benchmarking Plugin
Benchmarking used to calculate the power rating of the VM. 
### Configure
##### SSH Benchmarking Plugin
Calculo da potência da instância é determinado pelo tempo de execução de um script executado na instância via SSH. 

```bash
# Benchmarking class
benchmarking_class=org.fogbowcloud.manager.core.plugins.benchmarking.SSHBenchmarkingPlugin
# Benchmarking script to use with SSH Benchmarking plugin
ssh_benchmarking_script_url=http://downloads.fogbowcloud.org/benchmark/script_ssh_benchmarking.sh
# Manager public and private keys
ssh_private_key=/etc/fogbow-manager/ssh/id_rsa
ssh_public_key=/etc/fogbow-manager/ssh/id_rsa.pub
```
##### Vanilla Benchmarking Plugin
Calculo da potência da instância é determinado pela quantidade de VPUs e memória.

```bash
# Benchmarking class
benchmarking_class=org.fogbowcloud.manager.core.plugins.benchmarking.VanillaBenchmarkingPlugin
```

## Member Picker Plugin
Choice of a federation member that Fogbow Manager will order for resource.

### Configure
##### Round Robin Member Picker Plugin
O plugin escolhe o membro por ordem alfabética em uma lista.

```bash
# Member picker class
member_picker_class=org.fogbowcloud.manager.core.plugins.memberpicker.RoundRobinMemberPickerPlugin
```

##### NOF Member Picker Plugin
Este plugin usa o plugin do accounting para tomar a decisão de escolha do membro. Essa escolha é determinda com base no débito do membro da federação com o membro local. Quando maior o débido, mais proprício a ser escolhido.
```bash
# Member picker class
member_picker_class=org.fogbowcloud.manager.core.plugins.memberpicker.NoFMemberPickerPlugin
```

## Capacity controller plugin
### Configure
##### Fairness Driven Capacity Controller Plugin
```bash
```
##### Global Fairness Driven Controller Plugin
```bash
```
##### Hill Climbing Algorithm Plugin
```bash
```
##### Pairwise Fairnesse Driven Controller Plugin
```bash
```
##### Two Fold Capacity Controller Plugin
```bash
```
##### Satisfaction Driven Capacity Controller Plugin
```bash
```

## Mapper Plugin
Policy to map the federation user in one determinate user in the cloud. Esse mapeamento é feito com um identificador, que será determinado por um dos plugins, e as credenciais referentes ao plugin local de identidade. Quando não for possível encontrar o identificador via plugin é usado o identificador default obrigatoriamente.

Fórmula : 
mapper_ + {Identificador} + _ + {Credential}

```bash
# Openstack credentials: username, password, tenantName
# Identificador: defaults
mapper_defaults_username=fogbow
mapper_defaults_password=fogbow
mapper_defaults_tenantName=fogbow

# Identificador: other
# mapper_other_username=
# mapper_other_password=
# mapper_other_tenantName=

# Opennebula credentials: username, password
# Identificador: defaults
mapper_defaults_username=fogbow
mapper_defaults_password=fogbowpass

# EC2 credentals: accessKey, secretKey
# Identificador: defaults
mapper_defaults_accessKey=AKIALSKQLKFD7AHQLKEUO
mapper_defaults_secretKey=Iuaooiad&891/2309asd0123+akplkdh

# EC2 credentals: apiKey, secretKey
# Identificador: defaults
mapper_defaults_apiKey=user_api_key
mapper_defaults_secretKey=user_secret_key
```

### Configure
##### Federation User Based Mapper Plugin
O mapeamento é feito por intermédio do username logado como identificador.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.FederationUserBasedMapperPlugin

# Example 
# Identificador: fulado
mapper_fulano_username=fogbow
mapper_fulano_password=fogbow
mapper_fulano_tenantName=fogbow

# defaults
mapper_defaults_username=fogbowDefault
mapper_defaults_defaults_password=fogbowDefault
mapper_defaults_tenantName=fogbowDefaults
```
##### Member Based Mapper Plugin
O mapeamento é feito baseado no membro da federação que pediu o recurso.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.MemberBasedMapperPlugin

# Example 
# Identificador: manager.com.br
mapper_manager.com.br_username=fogbow
mapper_manager.com.br_password=fogbow
mapper_manager.com.br_tenantName=fogbow

# defaults
mapper_defaults_username=fogbowDefault
mapper_defaults_defaults_password=fogbowDefault
mapper_defaults_tenantName=fogbowDefaults
```
##### Simple Mapper Plugin
Mapeamento apenas com o defaults.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.SingleMapperPlugin

# Example 
# Identificador: defaults
mapper_defaults_username=fogbow
mapper_defaults_defaults_password=fogbow
mapper_defaults_tenantName=fogbow
```
##### VOBased Mapper Plugin
O mapeamento é feito com o CN do usuário do token VOMS.

```bash
# Mapper class
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.VOBasedMapperPlugin

# Example 
# Identificador: CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR
mapper_CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR_username=fogbow
mapper_CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR_password=fogbow
mapper_CN->Fulano,OU->DSC,O->UFCG,O->UFFBrGridCA,O->ICPEDU,C->BR_tenantName=fogbow

# defaults
mapper_defaults_username=fogbowDefault
mapper_defaults_defaults_password=fogbowDefault
mapper_defaults_tenantName=fogbowDefaults
```

## Prioritization Plugin
The Prioritization Plugin is responsible for prioritizing a request over another with lower priority.
It only comes into play when the quota for creating new resources is exceeded.
In these cases, it must verify if in that given time there is any fulfilled/ongoing request from a member with lower priority than the new requester.
If this condition is true, that request must preempted so that the new request can be met.
### Configure
##### Priotize Remove Order Plugin
```bash
```
##### Two Fold Prioritization Plugin
This plugin allows a more refined prioritization policy by allowing that two other plugins be used by composition.
One is used for prioritization of locar orders, and the other for prioritization of remote orders.
```bash
```
##### Nof Prioritization Plugin
This plugin uses the Network of Favors as policy. Roughly, the Network of Favors prioritizes members from which it owes more favors.
```bash
remote_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.nof.NoFPrioritizationPlugin
nof_prioritize_local=true
nof_trustworthy=false
```
##### FCFS Prioritization Plugin
This plugin doesn't perform any prioritization. The first request to come is the first to be served. 
Obviously, the request is only met if there is free quota.
```bash
local_prioritization_plugin_class=org.fogbowcloud.manager.core.plugins.prioritization.fcfs.FCFSPrioritizationPlugin
## Remote Prioritization Plugin

```
