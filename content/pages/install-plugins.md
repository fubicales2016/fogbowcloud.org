Title: Plugins
url: install-plugins
save_as: install-plugins.html
section: install
index: 1

# Plugins

The manager component was designed to be agnostic to the underlying cloud technology. There is a plugin layer between this component and the cloud, in a sense that plugins are responsible for translating fogbow requests to what the underlying cloud understands. Plugins are instantiated via reflection, and fully configured via the configuration file. Currently, fogbowcloud project make available some plugins, and you can feel free to implement new ones.

There three different categories of plugin: the **Identity Plugins**, the **Authorization Plugins** and the **Compute Plugins**.

## Identity Plugin

The Identity Plugin is responsible for getting and authenticating tokens for local and federation cloud users. The Local Identity Provider and the Federation Identity Provider must be the same when the authentication is done at the Local Cloud Identity Provider. Otherwise, you can configure different plugins for local and federation identity providers. 

### Configure

As you can see at the [Manager Install Guide](http://www.fogbowcloud.org/install-manager), when the installation is done, rename the file ```manager.conf.example``` to ```manager.conf```. You need to configure the Identity Plugin according to the Identity Provider you are using. The following sections go through the configuration for the Identity Plugins that come along with the Fogbow Manager. You can configure different plugins for local and federation identity providers.

**Keystone Identity Plugin** 

```bash

# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.openstack.KeystoneIdentityPlugin

# Cloud Identity endpoint
local_identity_url=http://localhost:5000

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.openstack.KeystoneIdentityPlugin

# Federation identity endpoint
federation_identity_url=http://localhost:5000

# Proxy account for remote requests @ the local identity provider 
local_proxy_account_user_name=fogbow
# Password of such account
local_proxy_account_password=fogbow
# Tenant of such account
local_proxy_account_tenant_name=demo

```

**OpenNebula Identity Plugin**

```bash

# Local Identity plugin class
local_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin

# Cloud identity endpoint
local_identity_url=http://localhost:2633/RPC2

# Federation Identity plugin class
federation_identity_class=org.fogbowcloud.manager.core.plugins.opennebula.OpenNebulaIdentityPlugin

# Cloud identity endpoint
federation_identity_url=http://localhost:2633/RPC2

# Proxy account for remote requests @ the local identity provider 
local_proxy_account_user_name=fogbow
# Password of such account
local_proxy_account_password=fogbow

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
Note : In the implementation of VOMS Plugin was used the [VOMS Api Java](https://github.com/italiangrid/voms-api-java). This Api does not support the JVM of oracle.

## Compute Plugin

The Compute Plugin is responsible for requesting, getting, and deleting instances at the local cloud. Different plugins can require different information depending on their implementation. Fogbow manager assumes that all cloud users have quota defined and all information at ```manager.conf``` file are correct. If not, the behaviour of federation may not be the expected.

### Configure

As you can see at the [Manager Install Guide](http://www.fogbowcloud.org/install-manager), after installation move the file ```manager.conf.example``` to ```manager.conf```. You need to add the compute plugin contents according to plugin that will be used. These examples show the required properties for plugins already available on fogbowcloud project. 

**OpenStack OCCI Compute Plugin**

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
compute_occi_image_fogbow-linux-x86=cadf2e29-7216-4a5e-9364-cf6513d5f1fd

# Network ID (This property is required only if user project has more than one network available)
# Example:
compute_occi_network_id=ea51ed0c-0e8a-448d-8202-c79777109ffe
```

**OpenStack Nova V2 Compute Plugin**
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

# Identifier for the fogbow-linux-x86 image (you can have several of these)
compute_novav2_image_fogbow-linux-x86=cadf2e29-7216-4a5e-9364-cf6513d5f1fd

# Network id (This property is required only if user project has more than one network available)
compute_novav2_network_id=ea51ed0c-0e8a-448d-8202-c79777109ffe
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
compute_one_image_fogbow-linux-x86=0

```

### About Image Property

As you can see above, you can configure the fogbow manager with as many image as you want. Therefore, each manager can be configured with different images and/or same images but different names. Currently, fogbow uses the global image names for requests, then it is interesting to exist as equal image name as possible between the managers. For example, if the user requests for one instance of **image-ubuntu** in cloud A and that cloud does not have available resource, the request is passed on to another cloud, cloud B, and will be fulfilled only if cloud B manager was configured with **image-ubuntu** (even if the cloud has the same image configured with different name, **image-ubuntu1204**, the request is not fulfilled). That is the reason we provide the most commom image names used by current fogbow installations:

 * **fogbow-linux-x86**: Cirros 0.3.3 image; cirros as username; and cubswin:) as password
 * **fogbow-ubuntu-12.04-with-java**: Ubuntu 12.04 image (standard ubuntu cloud image with java); ubuntu as username; password defined by admin

Furthermore, Fogbow also expects that all images configured at manager have **cloud-init** working properly.

## Authorization Plugin

The Authorization Plugin tells whether a given user (with a proper authenticated token) is able to create requests in the Fedartion. 

### Configure

The **federation_authorization_class** property must be set to a Authorization Plugin implementation. The Fogbow Manager comes with a single implementation that simply authorizes any user/token.

**Allow All Authorization Plugin**

```bash
# Federation Authorization plugin class
# Example 
federation_authorization_class=org.fogbowcloud.manager.core.plugins.common.AllowAllAuthorizationPlugin
```
