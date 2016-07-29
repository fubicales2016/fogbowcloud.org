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
After the installation, edit the file ```manager.conf```. In this file, some general properties, such as XMPP addresses and ports, as well as the set of plugins that define the behaviour of the Fogbow Manager are specified. Some of these properties need to be adjusted to match your particular cloud infrastructure provider. To ease the configuration procedure, we distribute template files for each cloud provider currenlty supported by the Fogbow middleware: ```manager.conf.cloudstack.example```,```manager.conf.opennebula.example```,```manager.conf.openstack.example``` and ```manager.conf.azure.example```. In this document, we cover in more details the general properties that might need to be changed and the main cloud-specific plugin properties. A complete overview of the Fogbow Manager plugins is given in the <a  href="/install-configure-plugins" target="_blank">Configure Fogbow Manager's Plugins </a> section of our documentation.

### General properties

The XMPP properties below indicate the XMMP server that the **Fogbow Manager** must contact to communicate in the federation. These properties also indicate the **Fogbow Rendezvous** associated with the **Fogbow Manager**.

```bash
# jid of the Fogbow Manager XMPP component
xmpp_jid=my-manager.internal.mydomain

# password of the Fogbow Manager XMPP component
xmpp_password=manager_password

# XMPP server IP address
xmpp_host=150.1.1.1

# Port in which the XMPP server will be listening.
xmpp_port=5347

# jid of your Rendezvous XMPP component
rendezvous_jid=my-site.rendezvous.com
```

The **xmpp_jid**, **xmpp_password** and **rendezvous_jid** properties above must match the values assigned during the <a  href="/install-configure-xmpp" target="_blank">Install and configure XMPP </a> section of our documentation.

Following, you need to add a new entry in your DNS to resolve the give **Fogbow Manager** component name to an IP address like in the example below.

``` shell
my-manager.internal.mydomain        22      IN      A       150.1.1.2
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

## Run 
To start the manager component, run the start-manager script inside ```bin```.

```bash
./bin/start-manager
```
