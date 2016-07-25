Title: Install and configure fogbow rendezvous
url: install-configure-rendezvous
save_as: install-configure-rendezvous.html
section: install-configure
index: 6

Install and configure the Fogbow Rendezvous
==========

The Fogbow Rendevouz is distributed in two forms: as source code or as a binary package for debian-based distributions. Choose the best distribution for your system, download it and install it as follow.

## Install from source
To get the lastest stable version of the component, download it from our repository

``` shell
wget https://github.com/fogbow/fogbow-rendezvous/archive/master.zip
```

Then, decompress it:
``` shell
unzip master.zip
```

Now, install it with Maven:

```
cd fogbow-rendezvous-master
mvn install
```

## Install from debian package

Download a stable version from our <a href="http://downloads.fogbowcloud.org/stable/debian/">package repository</a> and install it with dpkg:

```bash
dpkg -i fogbow-rendezvous_$version.deb
```

## Configure
After the installation, move the file ```rendezvous.conf.example``` to ```rendezvous.conf``` and edit its contents:
``` shell
# XMPP address of your Rendezvous Component
xmpp_jid=my-rendezvous.internal.mydomain

# XMPP component password
xmpp_password=password

# Address of the host running the XMPP server.
xmpp_host=127.0.0.1

# XMPP server port (to listen for components communication)
xmpp_port=5347

# The neighbors property indicates other existent rendezvous
# which this component can exchange information about Fogbow Managers liveness.
neighbors=neighbor1@domain1.com,neighbor2@domain2.com

# The properties below are used to limit, in size, the rendezvous' responses.
# Maximum number of managers that should be returned in a whoIsAlive response.
max_whoisalive_manager_count=100
# Maximum number of managers that should be returned in a whoIsAliveSyncresponse.
max_whoisalivesync_manager_count=100
# Maximum number of neighbors that should be returned in a whoIsAliveSyncresponse.
max_whoisalivesync_neighbor_count=100

# The properties below control "iamalive" mechanism
# The period in milliseconds between successive "iamalive" messages.
iamalive_period=30000
# How many "iamalive" messages should be lost before the Fogbow Rendezvous suspects
# the Fogbow Manager is not available.
iamalive_max_message_lost=3

# The property below indicates the WhiteList pluging used by the rendevouz.
white_list_class=org.fogbowcloud.rendezvous.core.plugins.whitelist.AcceptAnyWhiteListPlugin
``` 

Since the rendezvous is a XMPP component, you need a XMPP server running and properly configured to be able to communicate to other members in the fogbow federation.

TODO: remover essa frase e adicionar link para xmpp config
We recommend <a href="https://prosody.im/" target="_blank">prosody</a> due to its ease of configuration.

TODO: talvez essa linha nao seja necessaria

If you are using Prosody, you can add a component to its configuration with:
``` shell
Component "rendezvous.test.com"
       component_secret = "password"
```
# Actions before configure
After adding the component to your XMPP server, you need to add a new entry in your DNS to resolve your component name to the XMPP server IP address, as shown in the example below.
``` shell
rendezvous.test.com.        22      IN      A       199.27.76.133
```


## Run
To start the rendezvous component, run the start-rendezvous script inside ```./bin```.
``` shell
./bin/start-rendezvous
```
