Title: Manager
url: install-manager
save_as: install-manager.html
section: install
index: 1

# Manager

The Manager is the federation service that should run on each federation member. It provides an extended OCCI API for final users and interacts with Rendezvous and other Managers.  

## Install
To set up Manager, first, get the latest code of the project.
```bash
git clone https://github.com/fogbow/fogbow-manager.git
```
Then, install it in your machine.
```bash
mvn install -D
```

## Configure
After the installation the user can configure a few properties. All sections listed below are part of the same file.

* **XMPP properties:** Manager and Rendezvous XMPP properties that will be used to comunication between components.  

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

* **Cloud Compute Information:** All compute information required by compute plugin that will be used. Different plugins can require different information depending on their implementation. This section shows properties required by OpenStack compute plugin (currently provided by fogbow).

```bash
# Compute plugin class
# Example:
compute_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackComputePlugin

# Cloud OCCI endpoint
# Example:
compute_openstack_occi_url=http://localhost:8182

# Cloud v2 compute endpoint
# Example:
compute_openstack_v2api_url=http://localhost:8182

# Associating local cloud flavors to fogbow flavors
# Small flavor
# Example:
compute_openstack_flavor_small=m1-small

# Medium flavor
# Example:
compute_openstack_flavor_medium=m1-medium

# Large flavor
# Example:
compute_openstack_flavor_large=m1-large

# Image
# Example:
compute_openstack_default_cirros_image=cadf2e29-7216-4a5e-9364-cf6513d5f1fd
```

* **Cloud Identity Information:** All identity information required by identity plugin that will be used. Cloud identity is used to get and authenticate tokens for local cloud users. This section shows properties required by OpenStack identity plugin (currently provided by fogbow). 

```bash

# Identity plugin class
# Example:
identity_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackIdentityPlugin

# Cloud identity endpoint
# Example:
identity_openstack_url=http://localhost:5000
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

* **Validator Member Information:** Validator member is used to define if the manager can receive or donate to another one. It is possible differents implementations for it, and each implementation can require specific properties (as idnetity and compute plugin). This section shows properties required by default member validator (all member can receive and donate resources to each other).

```bash
# Member validator class
# Example:
member_validator=org.fogbowcloud.manager.core.DefaultMemberValidator

# Member certificate file contains the certificate the should be used by the manager 
# Example:
cert_path=path/certificate/file
```

* **Times Properties:** Time configuration required by Manager Component.

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

* **SSH Tunnel Properties:** SSH properties are used to give connectivity for instances.

```bash
# IP address of shh tunnel host
# Example:
ssh_tunnel_host=10.0.0.1

# shh tunnel user is a valid user at tunnel host. This user must not requere password
# Example:
ssh_tunnel_user=fogbow

# shh tunnel port range defines set of ports that should be used to create reverse tunnels to the instances
# Example:
ssh_tunnel_port_range=50000:59999
```

* **Manager HTTP Port:** HTTP port which manager component endpoint will be listening.

```bash
# http port is the Manager Component endpoint port
# Example:
http_port = 8182

```
## Run
To start Manager, run the start-manager script in the bin directory.

```bash
./bin/start-manager
```
