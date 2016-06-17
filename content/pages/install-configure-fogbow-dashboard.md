Title: Install and configure Fogbow Dashboard
url: install-configure-fogbow-dashboard
save_as: install-configure-fogbow-dashboard.html
section: install-configure
index: 5

Fogbow Dashboard
==========
The Fogbow Dashboard is a web interface to the Fogbow Manager. It provides all the operations that can be performed through the [Fogbow CLI](http://www.fogbowcloud.org/fogbow-cli).

##Installation

TODO: adicionar uma frase de resumo. p.ex podemos dizer que o codigo sera baixado do repositorio do fogbow, e as demais configs

```bash
git clone https://github.com/fogbow/fogbow-dashboard.git
sudo apt-get install git python-dev python-virtualenv libssl-dev libffi-dev libxml2-dev libxslt1-dev
cd fogbow-dashboard
chmod 600 openstack_dashboard/local/.secret_key_store
chmod 600 openstack_dashboard/test/.secret_key_store
# Download libraries and run tests
./run_tests.sh
```

##Configure
After the installation, edit the ```openstack_dashboard/local/local_settings.py``` file to indicate the HTTP endpoint of the Fogbow Manager associated with the Fogbow Dashboard:

``` bash
# Fogbow Manager to be used.
FOGBOW_MANAGER_ENDPOINT = "http://localhost:8182"

# Endpoint Federation
# types : keystone, opennebula, voms, raw_opennebula, raw_keystone, shibboleth, simpletoken
FOGBOW_FEDERATION_AUTH_TYPE = 'keystone'
FOGBOW_FEDERATION_AUTH_ENDPOINT = 'http://localhost:5000' 

# Custom interface theme
# not required
CUSTOM_THEME = 'naf-theme'
```

##Run

``` bash
nohup ./run_tests.sh --runserver localhost:9000 &
```

The Fogbow Dashboard is based on the [horizon](https://github.com/openstack/horizon) dashboard. To know more about the Horizon project, for example how to deploy it in different environments, see the <a href="http://docs.openstack.org/developer/horizon/index.html" target=_blank>Horizon docs</a>.
