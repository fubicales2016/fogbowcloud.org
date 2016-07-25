Title: Install and configure fogbow manager
url: install-configure-fogbow-manager
save_as: install-configure-fogbow-manager.html
section: install-configure
index: 3

# Fogbow Manager

The Fogbow Manager is distributed in two forms: as source code or as a binary package for debian-based distributions. Choose the best distribution for your system, download it and install it as follow.

## Install from source
To get the lastest stable version of the component, download it from our repository:

```bash
wget https://github.com/fogbow/fogbow-manager/archive/master.zip
```

Then, decompress it:

```bash
unzip master.zip
```

Now, install it with Maven:

```bash
cd fogbow-manager
mvn install
```

## Install from debian package

Download a stable version from our <a href="http://downloads.fogbowcloud.org/stable/debian/">package repository</a> and install it with dpkg:

```bash
dpkg -i fogbow-manager_$version.deb
```

## Configure
After the installation, move the file ```manager.conf.example``` to ```manager.conf```. In this file, some general properties, such as XMPP addresses and ports, as well as the set of plugins that define the behaviour of the Fogbow Manager are specified. Some of these propoerties need to be adjusted to match your particular installation. In this document, we cover in more details the general properties that might need to be changed. The Fogbow Manager plugins are covered in the <a  href="/install-configure-plugins" target="_blank">Configure Fogbow Manager's Plugins </a> section of our documentation.

### General properties

**XMPP information:**
As the Fogbow Manager runs as an XMPP component, it needs to access an XMPP server. You need to define some XMPP properties to allow the Fogbow Manager to communicate with the other federation members. Here is an example of the Fogbow Manager XMPP properties, considering the example XMPP installation described in the <a  href="/install-configure-xmpp" target="_blank">Install and configure XMPP </a> section of our documentation:

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

After configuring the Fogbow Manager, you need to add a new entry in your DNS to resolve your component name to an IP address like in the example below.

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

**SSH tunnel properties:** These properties configure the Fogbow Reverse Tunnel (FRT) service to provide public IP access to the VMs created by the Fogbow Manager in its intranet. The example below shows how these propoerties should be set, considering the example presented in the <a  href="/install-configure-frt" target="_blank">Install and configure Fogbow Reverse Tunnel </a> section of our documentation:

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

**Manager HTTP port:** This property indicates the HTTP port to which the Fogbow Manager component endpoint will be listening to requests.

```bash
# The http_port property indicates http port of the Fogbow Manager service endpoint
http_port = 8182
```

### Plugins
In this section, we overview the purpose of each plugin adopted by the Fogbow Manager. In the <a  href="/install-configure-plugins" target="_blank">Configure Fogbow Manager's Plugins </a> section of our documentation we provide a detailed description of all current implementations of each plugin, including their configuration. The example file comes with default values for all these plugins.

**Member validation plugin:** The Member validation plugin is used to the define to whom the Fogbow Manager can receive or donate resources.

**TODO:** Member validation refers to the **FederationMemberAuthorizationPlugin** and is specified via the property **member_validator_class**. Should we change the doc (or the code) to have a consistent description. For example, saying we have a **Federation Member authorization** plugin.

**[Cloud compute plugin](http://www.fogbowcloud.org/customazing-deployment#compute-plugin):** The Cloud compute plugin deals with the allocation, monitoring and management of computing resources in the cloud infrastructure. 

**Cloud identity information:** All identity information required by the identity plugin that will be used. You can see the required information by each identity plugin provided by fogbow at [Plugins Page](http://www.fogbowcloud.org/customazing-deployment).

**[Cloud storage plugin](http://www.fogbowcloud.org/customazing-deployment#storage-plugin):**

**[Cloud network plugin](http://www.fogbowcloud.org/customazing-deployment#network-plugin):**

**[Manager authorization plugin](http://www.fogbowcloud.org/customazing-deployment#authorization-plugin):**

**[Member picker plugin](http://www.fogbowcloud.org/customazing-deployment#authorization-plugin#member-picker-plugin):** Choice of a federation member that Fogbow Manager will order for resource.

**[Mapper user plugin](http://www.fogbowcloud.org/customazing-deployment#mapper-plugin):** Policy to map the user in one determinate project in the cloud.

**[Compute accounting plugin](http://www.fogbowcloud.org/customazing-deployment#accounting-plugin):** Stores compute accounting information. You can see the required information by each mapper user plugin provided by fogbow at [Plugins Page].

**[Storage accounting plugin](http://www.fogbowcloud.org/customazing-deployment#accounting-plugin):** Stores compute accounting information.
 
**Benchmarking plugin:** Benchmarking used to calculate the power rating of the VM.

By default , we are using the Vanilla Benchmarking Accounting Plugin. Access [Simple Storage Accounting Plugin  section](http://www.fogbowcloud.org/customazing-deployment#simple-storage-accounting-plugin) for more information.

**Image configuration:** Image to be used with instances created via manager. The image used need to have cloud init in order to be able to set machine configuration such as network, credential keys and machine name.

**[Image storage plugin](http://www.fogbowcloud.org/customazing-deployment#image-storage):**  Get images by image storage plugin.

**Static mapping imagens:** Static mapping between local image ids and image names. Applies to all image storage plugins

## Run 
To start the manager component, run the start-manager script inside ```bin```.

```bash
./bin/start-manager
```
