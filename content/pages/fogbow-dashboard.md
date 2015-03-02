Title: Fogbow Dashboard
url: fogbow-dashboard
save_as: fogbow-dashboard.html
section: usage
index: 1

Fogbow Dashboard
==========
The Fogbow Dashboard is an implemetation of a [horizon](https://github.com/openstack/horizon) dashboard and provides a web based user interface to the Fogbow Manager. The operations that can be performed via the Fogbow Dashboard are the same provided by the [Fogbow CLI](http://www.fogbowcloud.org/fogbow-cli).

##Installation

```bash
git clone https://github.com/fogbow/fogbow-dashboard.git
sudo apt-get install git python-dev python-virtualenv libssl-dev libffi-dev libxml2-dev libxslt1-dev
cd fogbow-dashboard
# Download libraries and run tests
./run_tests.sh
```

##Configure
After the installation, edit the ```openstack_dashboard/local/local_settings.py``` file to point the Fogbow Dashboard to the HTTP endpoint of a Fogbow Manager:

``` bash
# Fogbow Manager to be used.
FOGBOW_MANAGER_ENDPOINT = "http://localhost:8182"

# Endpoint Local 
FOGBOW_LOCAL_AUTH_ENDPOINT = 'http://localhost:5000'
# types : keystone, opennebula, token
FOGBOW_LOCAL_AUTH_TYPE = 'keystone'

# Endpoint Federation 
FOGBOW_FEDERATION_AUTH_ENDPOINT = 'http://localhost:5000' 
# types : keystone, opennebula, voms, token
FOGBOW_FEDERATION_AUTH_TYPE = 'voms'
```

##Run
``` bash
./run_tests.sh --runserver localhost:9000
```

To know more about the Horizon project, and how to deploy it within different environments, see the [Horizon docs](http://docs.openstack.org/developer/horizon/index.html).
