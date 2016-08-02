Title: Install and configure Fogbow Dashboard
url: install-configure-fogbow-dashboard
save_as: install-configure-fogbow-dashboard.html
section: install-configure
index: 5

Fogbow Dashboard
==========
The Fogbow Dashboard is a web interface to the Fogbow Manager. It provides all the operations that can be performed through the [Fogbow CLI](http://www.fogbowcloud.org/fogbow-cli).

##Installation

Before installing the Fogbow Dashboard, we need to install its depencies. In a debian-based distribution, this can be done as follow:

```bash
sudo apt-get install git python-dev python-virtualenv libssl-dev libffi-dev libxml2-dev libxslt1-dev
```

To install the Fogbow Dashboard, download its lastest stable version from our repository:

``` shell
wget https://github.com/fogbow/fogbow-dashboard/archive/master.zip
```

Then, decompress it:
``` shell
unzip master.zip
```

Now, install it with Maven:

```
cd fogbow-dashboard-master
mvn install
```

##Configure
After the installation, assign the proper permissions to the **keystore** files

```bash
cd fogbow-dashboard
chmod 600 openstack_dashboard/local/.secret_key_store
chmod 600 openstack_dashboard/test/.secret_key_store
```

then, edit the ```openstack_dashboard/local/local_settings.py``` file to indicate the HTTP endpoint of the Fogbow Manager associated with the Fogbow Dashboard (here we consider that the Fogbow Dashboard and the Fogbow Manager are installed in the same host):

``` python
# Fogbow Manager to be used.
FOGBOW_MANAGER_ENDPOINT = "http://localhost:8182"

FOGBOW_FEDERATION_AUTH_TYPE = 'keystone'
FOGBOW_FEDERATION_AUTH_ENDPOINT = 'http://localhost:5000' 
```

##Run
To start the Fogbow Dashboard, run the **run_tests.sh** script indicating the port that the dashboard will handle requests (**9000**).

``` bash
nohup ./run_tests.sh --runserver localhost:9000 &
```
