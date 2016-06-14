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
# Example:
rendezvous_jid=rendezvous.test.com
```
* **Manager Datastore Information:** Database that stores orders.
``` shell
manager_datastore_url=jdbc:sqlite:/tmp/dbManagerSQLite.db
```

* **Instance Federated Datastore Information:** Stores instance federated.
``` shell
instance_datastore_url=jdbc:sqlite:/tmp/federated_instance
```
* **Instance Federated Datastore Information:** Stores storage federated.
``` shell
storage_datastore_url=jdbc:sqlite:/tmp/federated_storage
```
* **Instance Federated Datastore Information:** Stores network federated.
``` shell
network_datastore_url=jdbc:sqlite:/tmp/federated_network
```

* **Cloud Compute Information:** All compute information required by the compute plugins that will be used. You can see the required information by each compute plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).

By default, we are using openstack plugin with example.
``` shell
compute_class=org.fogbowcloud.manager.core.plugins.compute.openstack.OpenStackNovaV2ComputePlugin
compute_novav2_url=http://localhost:8774
compute_glancev2_url=http://localhost:9292
compute_glancev2_image_visibility=private
compute_novav2_network_id=ea51ed0c-0e8a-448d-8202-c79777109ffe
```

* **Cloud Identity Information:** All identity information required by the identity plugin that will be used. You can see the required information by each identity plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).

By default, we are using openstack plugin with example.
``` shell
local_identity_class=org.fogbowcloud.manager.core.plugins.identity.openstack.KeystoneIdentityPlugin
local_identity_url=http://localhost:5000
```

* **Cloud Storage Information:** All storage information required by the storage plugins that will be used. You can see the required information by each storage plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).

By default, we are using openstack plugin with example.
``` shell
storage_class=org.fogbowcloud.manager.core.plugins.storage.openstack.OpenStackV2StoragePlugin
storage_v2_url=http://localhost:8776
```

* **Cloud Network Information:** All network information required by the network plugins that will be used. You can see the required information by each network plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).

By default, we are using openstack plugin with example.
``` shell
network_class=org.fogbowcloud.manager.core.plugins.network.openstack.OpenStackV2NetworkPlugin
network_openstack_v2_url=http://localhost:9696
external_gateway_info=ea51ed0c-0e8a-448d-8202-c79777109ffe
```

* **Cloud Authorization Information:** Cloud authorization is used to get the authorization in the federation.  You can see the required information by each authorization plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).

By default, we are using the plugin that authorize all users.
``` shell
federation_authorization_class=org.fogbowcloud.manager.core.plugins.authorization.AllowAllAuthorizationPlugin
```
* **Member picker Information:** Choice of a federation member that Fogbow Manager will order for resource.
By default, we are using the plugin that choose in a list.
``` shell
member_picker_class=org.fogbowcloud.manager.core.plugins.memberpicker.RoundRobinMemberPickerPlugin
```

* **Mapper User Information:** Policy to map the user in one determinate project in the cloud. You can see the required information by each mapper user plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).
 
By default, we are the plugin that map any user for one unique project.
``` shell
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.SingleMapperPlugin

mapper_defaults_username=fogbow
mapper_defaults_password=fogbow-pass
mapper_defaults_tenantName=fogbow-project
```

* **Validator Member Information:** Validator member is used to define if the manager can receive or donate to another one. It is possible different implementations for it, and each implementation can require specific properties (as identity and compute plugins). This section shows properties required by the default member validator (all members can receive and donate resources to each other).

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

* **SSH Tunnel Properties:** SSH properties are used to provide connectivity to instances.

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
ssh_tunnel_host_http_port=2223

```

* **Manager HTTP Port:** HTTP port to which the manager component endpoint will be listening. In order to add the manager to federation and make it available from outside the local network, add the manager http port to your firewall.

```bash
# http port is the Manager Component endpoint port
# Example:
http_port = 8182

```

* **Image configuration:** Image to be used with instances created via manager. The image used need to have cloud init in order to be able to set machine configuration such as network, credential keys and machine name.

## Run 
To start the manager component, run the start-manager script inside ```bin```.

```bash
./bin/start-manager
```
