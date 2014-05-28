Title: Manager
url: install-manager
save_as: install-manager.html
section: install
index: 1

# Manager

Bla bla. 

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
After the installation the user can configure a few properties listed below:
```bash

# jid of your Manager Component
# Example:
xmpp_jid=manager.test.com

# Component password
#Example:
xmpp_password=password

# ip aderess.
# Example:
xmpp_host=127.0.0.1

# port in which the server will be listening.
# Example:
xmpp_port=5347

# jid of your Rendezvous Component
# Example:
rendezvous_jid=rendezvous.test.com

# compute plugin class
# Example:
compute_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackComputePlugin

# endpoint occi
# Example:
compute_openstack_occi_url=http://localhost:8182

# endpoint v2api
# Example:
compute_openstack_v2api_url=http://localhost:8182

# small falvor
# Example:
compute_openstack_flavor_small=m1-small

# medium falvor
# Example:
compute_openstack_flavor_medium=m1-medium

# large falvor
# Example:
compute_openstack_flavor_large=m1-large

# image
# Example:
compute_openstack_default_cirros_image=cadf2e29-7216-4a5e-9364-cf6513d5f1fd

# identity plugin class
# Example:
identity_class=org.fogbowcloud.manager.core.plugins.openstack.OpenStackIdentityPlugin

# identity plugin endpoint
# Example:
identity_openstack_url=http://localhost:5000

# federation member name
# Example:
federation_user_name=fogbow

# federation member password
# Example:
federation_user_password=fogbow

# federation member tenant name
# Example:
federation_user_tenant_name=demo

# default member validator class
# Example:
member_validator=org.fogbowcloud.manager.core.DefaultMemberValidator

# certificate path 
# Example:
cert_path=

# scheduler period
# Example:
scheduler_period=30000

# tima token update
# Example:
token_update_period=300000

# time instance monitoruing period
# Example:
instance_monitoring_period=120000

# shh tunnel host
# Example:
ssh_tunnel_host=10.0.0.1

# shh tunnel user
# Example:
ssh_tunnel_user=fogbow

# shh tunnel port range
# Example:
ssh_tunnel_port_range=50000:59999

# http port
# Example:
http_port = 8182

```
## Run
To start Manager, run the start-manager script in the bin directory.
``` shell
./bin/start-manager
```
