Title: Manager
url: install-manager
save_as: install-manager.html
section: install
index: 1

# Manager

The manager is the fogbow's component that runs in each federation member. It provides a OCCI API for end users and interacts with the rendezvous and other managers. 

This tutorial assumes you have Openstack's OCCI API enabled in your underlying cloud setup. Please read the [Openstack's OCCI guide](https://wiki.openstack.org/wiki/Occi#How_to_use_the_OCCI_interface) to know more.

Also, the manager needs a user registered in the underlying private cloud in order to proxy remote requests to its local resources. For Openstack, you can add new users and projects as is described in the [Openstack Ops guide](http://docs.openstack.org/trunk/openstack-ops/content/projects_users.html#create_new_users). The configuration section of this page explains it in more detail.

## Install
To set up a manager instance, first, get the latest code from github.
```bash
git clone https://github.com/fogbow/fogbow-manager.git
```
Then, install it with Maven
```bash
mvn install
```

## Configure
After the installation, move the file ```manager.conf.example``` to ```manager.conf``` and edit its contents:

* **XMPP properties:** Manager and Rendezvous XMPP properties that will be used for the comunication between components.  

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

* **Cloud Compute Information:** All compute information required by the compute plugins that will be used. Different plugins can require different information depending on their implementation. This section shows properties required by OpenStack compute plugin (currently the only one provided by fogbow).

```bash
# Compute plugin class
# Example:
compute_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackComputePlugin

# Cloud OCCI endpoint
# Example:
compute_occi_url=http://localhost:8182

# Cloud v2 compute endpoint
# Example:
compute_openstack_v2api_url=http://localhost:8182

# Associating local cloud flavors to fogbow flavors
# Small flavor
# Example:
compute_flavor_small=m1-small

# Medium flavor
# Example:
compute_flavor_medium=m1-medium

# Large flavor
# Example:
compute_flavor_large=m1-large

# OS Cloud Scheme
# Example:
compute_occi_os_scheme=http://schemas.openstack.org/template/os#

# Instance Cloud Scheme
# Example:
compute_occi_instance_scheme=http://schemas.openstack.org/compute/instance#

# Resource Cloud Scheme
# Example:
compute_occi_resource_scheme=http://schemas.openstack.org/template/resource#

# Image (property format: compute_occi_image_image-name)
# Example (this image will be referenced as linuxx86):
compute_occi_image_linuxx86=cadf2e29-7216-4a5e-9364-cf6513d5f1fd

# Network ID (This property is required only if user project has more than one network available)
# Example:
compute_occi_network_id=ea51ed0c-0e8a-448d-8202-c79777109ffe
```

* **Cloud Identity Information:** All identity information required by the identity plugin that will be used. Cloud identity is used to get and authenticate tokens for local and federation cloud users. This section shows properties required by OpenStack identity plugin (currently the only one provided by fogbow). Note that the Local Identity Provider and the Federation Identity Provider are the same, this happens when the authentication is done at the Local Cloud Identity Provider.

```bash

# Local Identity plugin class
# Example:
local_identity_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackIdentityPlugin

# Cloud identity endpoint
# Example:
local_identity_url=http://localhost:5000

# Federation Identity plugin class
# Example:
federation_identity_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackIdentityPlugin

# Cloud identity endpoint
# Example:
federation_identity_url=http://localhost:5000
```

* **Federation User Information:** Cloud federation user is a local cloud user that will submit requests from remote members.

```bash
# Federation user name
# Example:
federation_user_name=fogbow

# Federation user password
# Example:
federation_user_password=fogbow

# Federation user tenant (project) name
# Example:
federation_user_tenant_name=demo
```

* **Validator Member Information:** Validator member is used to define if the manager can receive or donate to another one. It is possible different implementations for it, and each implementation can require specific properties (as identity and compute plugins). This section shows properties required by the default member validator (all members can receive and donate resources to each other).

```bash
# Member validator class
# Example:
member_validator=org.fogbowcloud.manager.core.DefaultMemberValidator

# Member certificate file contains the certificate the should be used by the manager 
# Example:
cert_path=path/certificate/file
```

* **Times Properties:** Time configuration required by the manager component.

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

# shh tunnel user is a valid user at tunnel host. This user must not requere password
# Example:
ssh_tunnel_user=fogbow

# shh tunnel port range defines set of ports that should be used to create reverse tunnels to the instances
# Example:
ssh_tunnel_port_range=50000:59999

# shh tunnel host port defines the port to be used when doing ssh. If this property isn't set, the default value is 22
# Example:
ssh_tunnel_host_port=22

```

* **Manager HTTP Port:** HTTP port to which the manager component endpoint will be listening.

```bash
# http port is the Manager Component endpoint port
# Example:
http_port = 8182

```
## Run
To start the manager component, run the start-manager script inside ```bin```.

```bash
./bin/start-manager
```