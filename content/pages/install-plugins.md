Title: Plugins
url: install-plugins
save_as: install-plugins.html
section: install
index: 1

# Plugins

The manager component was designed to be agnostic to the underlying cloud technology. There is a plugin layer between this component and the cloud, in a sense that plugins are responsible for translating fogbow requests to what the underlying cloud understands. Plugins are instantiated via reflection, and fully configured via the configuration file. Currently, fogbowcloud project make available some plugins, and you can feel free to implement new ones.

The plugins can be classified into two categories: **Identity Plugin** and **Compute Plugin**.

## Identity Plugin

The Identity Plugin is responsible for getting and authenticating tokens for local and federation cloud users. The Local Identity Provider and the Federation Identity Provider must be the same when the authentication is done at the Local Cloud Identity Provider. Otherwise, you can configure different plugins for local and federation identity providers. 

### Configure

As you can see at the [Manager Install Guide](http://www.fogbowcloud.org/install-manager), after installation move the file ```manager.conf.example``` to ```manager.conf```. You need to add the identity plugin contents according to plugin that will be used. These examples show the required properties for plugins already available on fogbowcloud project. You can configure different plugins for local and federation idnetity providers.

**OpenStack Idnetity Plugin** 

```bash

# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackIdentityPlugin

# Cloud identity endpoint
local_identity_url=http://localhost:5000

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackIdentityPlugin

# Cloud identity endpoint
federation_identity_url=http://localhost:5000

```

**OpenNebula Idnetity Plugin**

```bash

# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin

# Cloud identity endpoint
local_identity_url=http://localhost:2633/RPC2

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin

# Cloud identity endpoint
federation_identity_url=http://localhost:2633/RPC2

```

**X509 Plugin**

```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.x509.X509IdentityPlugin

# Directory where are the certificates of Certificate Authorities (CA). They are certificates that you trust.
x509_ca_dir_path=/path/to/ca/directory

```

**VOMs Plugin**

```bash
# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.voms.VomsIdentityPlugin

# Directory where are the VOMS server information. 
# List of voms servers in order to issue a proxy. 
# Default : "~/.glite/vomses"
path_vomses=/path/vomes

# Directory where are the certificates of Certificate Authorities (CA). They are certificates that you trust.
# Default : "/etc/grid-security/certificates"
path_trust_anchors=/path/trust/anchors

# Directory where are the certificates of VOMS that you trust.
# Default : "/etc/grid-security/vomsdir"
path_vomsdir=/path/voms/dir

```

## Compute Plugin

The Compute Plugin is responsible for requesting, getting, and deleting instances at the local cloud. Different plugins can require different information depending on their implementation. Fogbow manager assumes that all cloud users have quota defined and all information at ```manager.conf``` file are correct. If not, the behaviour of federation may not be the expected.

### Configure

As you can see at the [Manager Install Guide](http://www.fogbowcloud.org/install-manager), after installation move the file ```manager.conf.example``` to ```manager.conf```. You need to add the compute plugin contents according to plugin that will be used. These examples show the required properties for plugins already available on fogbowcloud project. 

**OpenStack Compute Plugin**

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

# Image (property format: compute_occi_image_image-name). You can add as many image as you want.
# Example (this image will be referenced as linuxx86):
compute_occi_image_linuxx86=cadf2e29-7216-4a5e-9364-cf6513d5f1fd

# Network ID (This property is required only if user project has more than one network available)
# Example:
compute_occi_network_id=ea51ed0c-0e8a-448d-8202-c79777109ffe
```

**OpenNebula Compute Plugin**

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

# Image (property format: compute_one_image_image-name). You can add as many image as you want.
# Example (this image will be referenced as linuxx86):
compute_one_image_linuxx86=0

```

## Authorization Plugin

The Authorization Plugin is responsible for getting authorization with the federation. 

### Configure

As you can see at the [Manager Install Guide](http://www.fogbowcloud.org/install-manager), after installation move the file ```manager.conf.example``` to ```manager.conf```. You need to add the authorization plugin contents according to plugin that will be used. These examples show the required properties for plugins already available on fogbowcloud project. 

**Allow All Authorization Plugin**

```bash
# Federation Authorization plugin class
# Example 
federation_authorization_class=org.fogbowcloud.manager.core.plugins.openstack.AllowAllAuthorizationPlugin
```
