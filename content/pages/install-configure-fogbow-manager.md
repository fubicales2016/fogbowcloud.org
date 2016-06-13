Title: Install and configure fogbow manager
url: install-configure-fogbow-manager
save_as: install-configure-fogbow-manager.html
section: install-configure
index: 3

# Manager

The manager is the fogbow's component that runs in each federation member. It provides a OCCI API for end users and interacts with the rendezvous and other managers. 

This tutorial assumes you have Openstack's OCCI API enabled in your underlying cloud setup. Please read the <a href="https://wiki.openstack.org/wiki/Occi#How_to_use_the_OCCI_interface" target=_blank>Openstack's OCCI guide</a> to know more.

Also, the manager needs a user registered in the underlying private cloud in order to proxy remote requests to its local resources. For Openstack, you can add new users and projects as is described in the <a href="http://docs.openstack.org/trunk/openstack-ops/content/projects_users.html#create_new_users" target=_blank>Openstack Ops guide</a>. The configuration section of this page explains it in more detail.

## Install from source
As the manger runs as an XMPP component, you need an XMPP server running and properly configured.
We recommend <a href="https://prosody.im/" target=_blank>prosody 0.9.8+</a> due to its ease of configuration.

If you are using Prosody, you can add a component to its configuration with:
``` shell
Component "manager.test.com"
       component_secret = "password"
```

After add the component to your XMPP server, you need to add a new entry in your DNS to resolve your component name to a IP address like in example below. It is needed for all XMPP components such as rendezvous and Fogbow manager.
``` shell
manager.test.com.        22      IN      A       199.27.76.133
```

With an XMPP server already installed and configured, get the latest code of the project.
```bash
git clone https://github.com/fogbow/fogbow-manager.git
```
Then, install it with Maven
```bash
mvn install
```

## Install from debian package
To set up a manager instance, first, download to the <a href="http://downloads.fogbowcloud.org/stable/debian/v0.2.2/fogbow-manager/fogbow-manager_v0.2.2.deb">latest debian package</a>
```bash
wget http://downloads.fogbowcloud.org/stable/debian/v0.2.2/fogbow-manager/fogbow-manager_v0.2.2.deb
```

Then, install it with dkpg
```bash
dpkg -i fogbow-manager_v0.2.2.deb 
```

## Configure
After the installation, move the file ```manager.conf.example``` to ```manager.conf``` and edit its contents:

* **XMPP properties:** Manager and Rendezvous XMPP properties that will be used for the comunication between components. These components are XMPP components and need to be added to the XMPP configuration in the components section, as mentioned in the "Install from source" section.

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

* **Cloud Compute Information:** All compute information required by the compute plugins that will be used. You can see the required information by each compute plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).


* **Cloud Identity Information:** All identity information required by the identity plugin that will be used. You can see the required information by each identity plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).


* **Cloud Authorization Information:** Cloud authorization is used to get the authorization in the federation.  You can see the required information by each authorization plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/install-plugins).


* **Local User Information:** Cloud local user is a local cloud user that will submit requests from remote members.

```bash
# Local user name
# Example:
local_proxy_account_user_name=fogbow

# Local user password
# Example:
local_proxy_account_password=fogbow

# Local user tenant (project) name
# Example:
local_proxy_account_tenant_name=demo
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
