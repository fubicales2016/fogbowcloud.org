Title: Install and configure fogbow manager
url: install-configure-fogbow-manager
save_as: install-configure-fogbow-manager.html
section: install-configure
index: 3

# Manager
TODO: manel, melhorar a descricao do manager. alinhar com o que escrevemos no big picture.

TODO: manel, verificar valores das propriedades, p.ex portas. lembrar que precisa ser consistente em toda a documentacao

The manager is the fogbow's component that runs in each federation member. It provides a OCCI API for end users and interacts with the rendezvous and other managers. 

## Install from source
Get the latest version of the source code
```bash
git clone https://github.com/fogbow/fogbow-manager.git
```
Then, install it with Maven
```bash
mvn install
```

## Install from debian package
To set up a Fogbow Manager instance, first, download the <a href="http://downloads.fogbowcloud.org/nightly/debian/fogbow-manager/fogbow-manager_latest.deb">latest debian package</a>
```bash
wget http://downloads.fogbowcloud.org/nightly/debian/fogbow-manager/fogbow-manager_latest.deb
```

Then, install it with dpkg
```bash
dpkg -i fogbow-manager_latest.deb
```

## Configure
After the installation, move the file ```manager.conf.example``` to ```manager.conf```. In this file, some general properties, such as XMPP addresses and ports, as well as the set of plugins that define the behaviour of the Fogbow Manager are specified. In this document, we cover in more details the general properties. The Fogbow Manager plugins are covered **here**.

**XMPP information:**
As the Fogbow Manager runs as an XMPP component, it needs to access an XMPP server. For more information about how to install and configure an XMPP server, read the <a  href="/install-configure-xmpp" target="_blank">Install and configure XMPP </a> session. After the installation of the XMPP server, you need to define some XMPP properties to allow the Fogbow Manager to communicate with the other federation members. Here is an example of the Fogbow Manager XMPP properties:

```bash
# jid of the Fogbow Manager XMPP component
xmpp_jid=my-site.manager.com

# password of the Fogbow Manager XMPP component
xmpp_password=password

# XMPP server IP address
xmpp_host=150.1.1.1

# Port in which the XMPP server will be listening.
xmpp_port=5347

# jid of your Rendezvous XMPP component
rendezvous_jid=my-site.rendezvous.com
```
It is important to note that some XMPP properties defined in this configuration file, such as the **xmpp_jid** and the **xmpp_password**, are meant to be used in the XMPP server configuration, as explained <a  href="/install-configure-xmpp" target="_blank">here</a>.

After configuring the Fogbow Manager, you need to add a new entry in your DNS to resolve your component name to a IP address like in example below. It is needed for all XMPP components such as rendezvous and Fogbow manager.
``` shell
my-site.manager.com        22      IN      A       150.1.1.2
```

**Time intervals:** The Fogbow Manager executes some background tasks, such as resource monitoring, periodically. All the time interval properties are defined in the class ```org.fogbowcloud.manager.core.ConfigurationConstants``` and have default period values specified in the class ```org.fogbowcloud.manager.core.ManagerController```. The default values of any property can be overwritten in the configuration file as shown below:

```bash
# The scheduler_period property is the time interval (in milliseconds) in which
# the Fogbow Manager submit requests that are not fulfilled yet
scheduler_period=30000

# The accounting_update_period is the time interval (in milliseconds) in which
# the Fogbow Manager calculate the resource usage accounting
accounting_update_period=300000
```

**SSH tunnel properties:** These properties configure the Fogbow Reverse Tunnel (FRT) service to provide public IP access to the VMs created by the Fogbow Manager in its intranet.

```bash
# The token_host_public_address property defines the public IP address of the FRT service
token_host_public_address=150.160.0.10

# The token_host_private_address property defines the private IP address of the FRT service
token_host_private_address=10.0.0.1

# The token_host_port property defines the port to be used when doing ssh.
# If this property isn't set, the default value is 2222
token_host_port=2222

# The token_host_http_port property defines the port to be used when communicating with the FRT
token_host_http_port=2223
```

**Manager HTTP port:** This property indicates the HTTP port to which the Fogbow Manager component endpoint will be listening to requests. Since the Fogbow Manager service should be available from outside the local network (when it joins a federation) the firewall in your organization must allow communication through this por.

```bash
# The http_port property indicates http port of the Fogbow Manager service endpoint
http_port = 8182
```

*TODO: introduzir que vamos iniciar a seção de plugins. falar que eles serão descritos em seus detalhes em outra seção. descrever que configurações típicas de plugins em alguns federações podem ser vistas em outras seções.*

**Member validation:** Member validation is used to the define to whom the Fogbow Manager can receive or donate resources. The example below shows the properties required by the default member validator (this default allow to receive and donate resource from/to any Fogbow Manager). 

**Manager datastore information:** The path to the sqlite database that stores orders.
``` shell
manager_datastore_url=jdbc:sqlite:/home/fogbow/db_manager_orders.db
```

**TODO melhorar essa descricao**

**Instance federated datastore information:** The path to the database that stores instance federated.
``` shell
instance_datastore_url=jdbc:sqlite:/home/fogbow/federated_instance
```

**TODO melhorar essa descricao**

**Instance federated datastore information:** The path to the database that stores storage federated.
``` shell
storage_datastore_url=jdbc:sqlite:/home/fogbow/federated_storage
```

**TODO melhorar essa descricao**

**Instance federated datastore information:** The path to the database that stores network federated.
``` shell
network_datastore_url=jdbc:sqlite:/home/fogbow/federated_network
```

**TODO: listar os plugins**

**Cloud compute information:** All compute information required by the compute plugins that will be used. You can see the required information by each compute plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment#compute-plugin).

**TODO: listar os plugins**

**Cloud identity information:** All identity information required by the identity plugin that will be used. You can see the required information by each identity plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment).

**TODO: listar os plugins**

**Cloud storage  information:** All storage information required by the storage plugins that will be used. You can see the required information by each storage plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment#storage-plugin).

**TODO: listar os plugins**

**Cloud network information:** All network information required by the network plugins that will be used. You can see the required information by each network plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment#network-plugin).

**TODO: listar os plugins**

**Manager authorization information:** Manager authorization is used to get the authorization in the federation.  You can see the required information by each authorization plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment#authorization-plugin).


**TODO: listar os plugins**

**Member picker information:** Choice of a federation member that Fogbow Manager will order for resource.  You can see the required information by each member picker plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment#authorization-plugin#member-picker-plugin).

**TODO: listar os plugins**

**Mapper user information:** Policy to map the user in one determinate project in the cloud. You can see the required information by each mapper user plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment#mapper-plugin).
 
By default, we are the plugin that map any user for one unique project. Access [Simple Mapper Plugin  session](http://www.fogbowcloud.org/customazing-deployment#simple-mapper-plugin) for more information.
``` shell
# Example:
federation_user_credentail_class=org.fogbowcloud.manager.core.plugins.localcredentails.SingleMapperPlugin

mapper_defaults_username=fogbow
mapper_defaults_password=fogbow-pass
mapper_defaults_tenantName=fogbow-project
```

**TODO: listar os plugins**

* **Compute accounting plugin configuration:** Stores compute accounting information. You can see the required information by each mapper user plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment#accounting-plugin).

By default , we are using the FCU Accounting Plugin. Access [FCU Accounting Plugin   session](http://www.fogbowcloud.org/customazing-deployment#fcu-accounting-plugin) for more information.
```bash
# default for accounting plugins
# the period for update of the accounting
accounting_update_period=300000

accounting_class=org.fogbowcloud.manager.core.plugins.accounting.FCUAccountingPlugin
# path to the database
fcu_accounting_datastore_url=jdbc:sqlite:/tmp/computeusage
```
* **Storage accounting plugin configuration:** Stores compute accounting information. You can see the required information by each mapper user plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment#accounting-plugin).
 
By default , we are using the Simple Storage Accounting Plugin. Access [Simple Storage Accouting Plugin  session](http://www.fogbowcloud.org/customazing-deployment#simple-storage-accounting-plugin) for more information.
```bash
# default for accounting plugins
# the period for update of the accounting
accounting_update_period=300000

storage_accounting_class=org.fogbowcloud.manager.core.plugins.accounting.SimpleStorageAccountingPlugin
# path to the database
simple_storage_accounting_datastore_url=jdbc:sqlite:/tmp/storageusage
```

* **Benchmarking configuration:** Benchmarking used to calculate the power rating of the VM.
By default, we are the plugin that determine the same power rating for any VM. You can see the required information by each mapper user plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment#benchmarking-plugin).

By default , we are using the Vanilla Benchmarking Accounting Plugin. Access [Simple Storage Accounting Plugin  session](http://www.fogbowcloud.org/customazing-deployment#simple-storage-accounting-plugin) for more information.
```bash
# Example:
benchmarking_class=org.fogbowcloud.manager.core.plugins.benchmarking.VanillaBenchmarkingPlugin
```

* **Image configuration:** Image to be used with instances created via manager. The image used need to have cloud init in order to be able to set machine configuration such as network, credential keys and machine name.

* **Image storage plugin configuration:**  Get images by image storage plugin. You can see the required information by each mapper user plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment#image-storage).

By default, we are the plugin that download the image. Access [HTTP Download Image Storage Plugin  session](http://www.fogbowcloud.org/customazing-deployment#http-download-image-storage-plugin) for more information.
```bash
# Example:
image_storage_class=org.fogbowcloud.manager.core.plugins.imagestorage.http.HTTPDownloadImageStoragePlugin
image_storage_http_base_url=http://appliance-repo.egi.eu/images
image_storage_http_tmp_storage=/tmp/
```

* **Static mapping imagens:** Static mapping between local image ids and image names. Applies to all image storage plugins
```bash
# Example:
image_storage_static_fogbow-linux-x86=55d938ef-57d1-44ea-8155-6036d170780a 
image_storage_static_fogbow-ubuntu-1204=81765250-a4e4-440d-a215-43c9c0849120

# fogbow-linux-x86 is the local name and your id is 55d938ef-57d1-44ea-8155-6036d170780a
# fogbow-ubuntu-1204 is the local name and your id is 81765250-a4e4-440d-a215-43c9c0849120
```

## Run 
To start the manager component, run the start-manager script inside ```bin```.

```bash
./bin/start-manager
```
