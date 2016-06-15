Title: Install and configure fogbow manager
url: install-configure-fogbow-manager
save_as: install-configure-fogbow-manager.html
section: install-configure
index: 3

# Manager

The manager is the fogbow's component that runs in each federation member. It provides a OCCI API for end users and interacts with the rendezvous and other managers. 

This tutorial assumes you have Openstack's OCCI API enabled in your underlying cloud setup. Please read the <a href="https://wiki.openstack.org/wiki/Occi#How_to_use_the_OCCI_interface" target="_blank">Openstack's OCCI guide</a> to know more.

Also, the manager needs a user registered in the underlying private cloud in order to proxy remote requests to its local resources. For Openstack, you can add new users and projects as is described in the <a href="http://docs.openstack.org/trunk/openstack-ops/content/projects_users.html#create_new_users" target="_blank">Openstack Ops guide</a>. The configuration section of this page explains it in more detail.

## Install from source
Get the latest code of the project.
```bash
git clone https://github.com/fogbow/fogbow-manager.git
```
Then, install it with Maven
```bash
mvn install
```

## Install from debian package
To set up a manager instance, first, download to the <a href="http://downloads.fogbowcloud.org/nightly/debian/fogbow-manager/fogbow-manager_latest.deb">latest debian package</a>
```bash
wget http://downloads.fogbowcloud.org/nightly/debian/fogbow-manager/fogbow-manager_latest.deb
```

Then, install it with dkpg
```bash
dpkg -i fogbow-manager_latest.deb
```

## Actions before configure
As the manager runs as an XMPP component, you need an XMPP server running and properly configured. For more information about how install e configure the XMPP, access <a  href="/install-configure-xmpp" target="_blank">Install and configure XMPP session</a>. For example:

If you are using Prosody, you can add a component to its configuration with:
``` shell
Component "manager.test.com"
       component_secret = "password"
```

After add the component to your XMPP server, you need to add a new entry in your DNS to resolve your component name to a IP address like in example below. It is needed for all XMPP components such as rendezvous and Fogbow manager.
``` shell
manager.test.com.        22      IN      A       199.27.76.133
```

## Configure
After the installation, move the file ```manager.conf.example``` to ```manager.conf``` and edit its contents:

* **XMPP properties:** Manager and Rendezvous XMPP properties that will be used for the comunication between components. These components are XMPP components and need to be added to the XMPP configuration in the components section, as mentioned in the <a  href="/install-configure-xmpp" target="_blank">Install and configure XMPP session</a>.

```bash
# jid of your Manager Component
# Example:
xmpp_jid=manager.test.com

# Component password
# Example:
xmpp_password=password

# IP address.
# Example:
xmpp_host=127.0.0.1

# Port in which the server will be listening.
# Example:
xmpp_port=5347

# jid of your Rendezvous Component
# Optional if not to enter federation
# Example:
rendezvous_jid=rendezvous.test.com

# jid of your Greensitter
# Optional
# Example:
greensitter_jid=greensitter.test.com
```

* **Manager datastore information:** Database that stores orders.
``` shell
# Example:
manager_datastore_url=jdbc:sqlite:/tmp/db_manager_orders.db
```

* **Instance federated datastore information:** Database that stores instance federated.
``` shell
# Example:
instance_datastore_url=jdbc:sqlite:/tmp/federated_instance
```
* **Instance federated datastore information:** Stores storage federated.
``` shell
# Example:
storage_datastore_url=jdbc:sqlite:/tmp/federated_storage
```
* **Instance federated datastore information:** Stores network federated.
``` shell
# Example:
network_datastore_url=jdbc:sqlite:/tmp/federated_network
```

* **Cloud compute information:** All compute information required by the compute plugins that will be used. You can see the required information by each compute plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment).

By default, we are using openstack plugin as example. Access [Customazing and Deployment session](http://www.fogbowcloud.org/customazing-deployment) for more information.
``` shell
# Example:
compute_class=org.fogbowcloud.manager.core.plugins.compute.openstack.OpenStackNovaV2ComputePlugin
compute_novav2_url=http://localhost:8774
compute_glancev2_url=http://localhost:9292
compute_glancev2_image_visibility=private
compute_novav2_network_id=ea51ed0c-0e8a-448d-8202-c79777109ffe
```

* **Cloud identity information:** All identity information required by the identity plugin that will be used. You can see the required information by each identity plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment).

By default, we are using openstack plugin as example. Access [Customazing and Deployment session](http://www.fogbowcloud.org/customazing-deployment) for more information.
``` shell
# Example:
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.openstack.KeystoneIdentityPlugin
local_identity_url=http://localhost:5000
```

* **Cloud storage  information:** All storage information required by the storage plugins that will be used. You can see the required information by each storage plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).

By default, we are using openstack plugin as example. Access [Customazing and Deployment session](http://www.fogbowcloud.org/customazing-deployment) for more information.
``` shell
# Example:
storage_class=org.fogbowcloud.manager.core.plugins.storage.openstack.OpenStackV2StoragePlugin
storage_v2_url=http://localhost:8776
```

* **Cloud network information:** All network information required by the network plugins that will be used. You can see the required information by each network plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).

By default, we are using openstack plugin as example. Access [Customazing and Deployment session](http://www.fogbowcloud.org/customazing-deployment) for more information.
``` shell
# Example:
network_class=org.fogbowcloud.manager.core.plugins.network.openstack.OpenStackV2NetworkPlugin
network_openstack_v2_url=http://localhost:9696
external_gateway_info=ea51ed0c-0e8a-448d-8202-c79777109ffe
```

* **Manager authorization information:** Manager authorization is used to get the authorization in the federation.  You can see the required information by each authorization plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment).

By default, we are using the plugin that authorize all users.
``` shell
# Example:
federation_authorization_class=org.fogbowcloud.manager.core.plugins.authorization.AllowAllAuthorizationPlugin
```
* **Member picker information:** Choice of a federation member that Fogbow Manager will order for resource.

By default , we are using the plugin que choose in the list by alphabetic order.
``` shell
# Example:
member_picker_class=org.fogbowcloud.manager.core.plugins.memberpicker.RoundRobinMemberPickerPlugin
```

* **Mapper user information:** Policy to map the user in one determinate project in the cloud. You can see the required information by each mapper user plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment).
 
By default, we are the plugin that map any user for one unique project.
``` shell
# Example:
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.SingleMapperPlugin

mapper_defaults_username=fogbow
mapper_defaults_password=fogbow-pass
mapper_defaults_tenantName=fogbow-project
```

* **Validator member information:** Validator member is used to define if the manager can receive or donate to another one. It is possible different implementations for it, and each implementation can require specific properties (as identity and compute plugins). This section shows properties required by the default member validator (all members can receive and donate resources to each other).

```bash
# Member validator class
# Example:
member_validator=org.fogbowcloud.manager.core.DefaultMemberValidator

# Member certificates authorities path
# Example
member_validator_ca_dir=

# Member certificate file contains the certificate the should be used by the manager 
# Example:
cert_path=path/certificate/file
```

* **Intervals:** Properties related to scheduling, token update and instance monitoring.

```bash
# Scheduler period is the interval of time that the Manager Component will periodicaly submit requests that are not fulfilled yet
# Uses miliseconds unit
# Example:
scheduler_period=30000

# Token update period is the interval of time that the Manager Component will check if it is needed to get new token for requests and get it if yes
# Uses miliseconds unit
# Example:
token_update_period=300000

# Instance monitoring period is the interval of time that the Manager Component will check if the request's instance still exists. If not, the manager will update request state according to request's attributes
# Uses miliseconds unit
# Example:
instance_monitoring_period=120000
```

* **SSH tunnel properties:** SSH properties are used to provide connectivity to instances.

```bash
# Public IP address of shh tunnel host
# Example:
ssh_tunnel_public_host=150.160.0.10

# Private IP address of shh tunnel host
# Example:
ssh_tunnel_private_host=10.0.0.1

# shh tunnel host port defines the port to be used when doing ssh. If this property isn't set, the default value is 2222
# Example:
ssh_tunnel_host_port=2222

# shh tunnel host http port defines the port to be used when doing comunication with que ssh tunnel host
# Example:
ssh_tunnel_host_http_port=2223wi
```

* **Manager HTTP port:** HTTP port to which the manager component endpoint will be listening. In order to add the manager to federation and make it available from outside the local network, add the manager http port to your firewall.

```bash
# http port is the Manager Component endpoint port
# Example:
http_port = 8182
```

* **Compute accounting plugin configuration:** Stores compute accounting information.
```bash
# default for accounting plugins
# the period for update of the accounting
accounting_update_period=300000

accounting_class=org.fogbowcloud.manager.core.plugins.accounting.FCUAccountingPlugin
# path to the database
fcu_accounting_datastore_url=jdbc:sqlite:/tmp/computeusage
```
* **Storage accounting plugin configuration:** Stores compute accounting information.
```bash
# default for accounting plugins
# the period for update of the accounting
accounting_update_period=300000

storage_accounting_class=org.fogbowcloud.manager.core.plugins.accounting.SimpleStorageAccountingPlugin
# path to the database
simple_storage_accounting_datastore_url=jdbc:sqlite:/tmp/storageusage
```

* **Benchmarking configuration:** Benchmarking used to calculate the power rating of the VM.
By default, we are the plugin that determine the same power rating for any VM.
```bash
# Example:
benchmarking_class=org.fogbowcloud.manager.core.plugins.benchmarking.VanillaBenchmarkingPlugin
```

* **Image configuration:** Image to be used with instances created via manager. The image used need to have cloud init in order to be able to set machine configuration such as network, credential keys and machine name.

* **Image storage plugin configuration:**  Get images by image storage plugin.
By default, we are the plugin that download the image.
```bash
# Example:
image_storage_class=org.fogbowcloud.manager.core.plugins.imagestorage.http.HTTPDownloadImageStoragePlugin
image_storage_http_base_url=http://appliance-repo.egi.eu/images
image_storage_http_tmp_storage=/tmp/
```

* **Static mapping imagens:** Static mapping between local image ids and image names. Applies to all image storage plugins
```bash
# Example:
image_storage_static_fogbow-linux-x86=55d938ef-57d1-44ea-8155-6036d170780a 
image_storage_static_fogbow-ubuntu-1204=81765250-a4e4-440d-a215-43c9c0849120

# fogbow-linux-x86 is the local name and your id is 55d938ef-57d1-44ea-8155-6036d170780a
# fogbow-ubuntu-1204 is the local name and your id is 81765250-a4e4-440d-a215-43c9c0849120
```

## Run 
To start the manager component, run the start-manager script inside ```bin```.

```bash
./bin/start-manager
```
