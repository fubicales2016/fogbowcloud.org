Title: Install and configure fogbow rendezvous
url: install-configure-rendezvous
save_as: install-configure-rendezvous.html
section: install-configure
index: 6

Install and configure rendezvous
==========

intro

The **Rendezvous** is the discovery service that allows fogbow members to find each other in a fogbow federation.

##Installation

## Install from source
Get the latest code of the project.
``` shell
git clone https://github.com/fogbow/fogbow-rendezvous.git
```
Then, install it
``` shell
mvn install
```

## Install from debian package
With an XMPP server already installed and configured, download to the <a href="http://downloads.fogbowcloud.org/nightly/debian/fogbow-rendezvous/fogbow-rendezvous_latest.deb" target=_blank>latest debian package</a>
```bash
wget http://downloads.fogbowcloud.org/nightly/debian/fogbow-rendezvous/fogbow-rendezvous_latest.deb
```

Then, install it with dkpg
```bash
dpkg -i fogbow-rendezvous_latest.deb
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

## Configure
After the installation, move the file ```rendezvous.conf.example``` to ```rendezvous.conf``` and edit its contents:
``` shell
# XMPP address of your Rendezvous Component
xmpp_jid=rendezvous.test.com

# XMPP component password
xmpp_password=password

# Address of the host running the XMPP server.
xmpp_host=127.0.0.1

# XMPP server port (to listen for components communication)
xmpp_port=5347
```

TODO: nao entendi "system crashes"

The **neighbors** property indicates other existent rendezvous which this component can exchange information about **Fogbow Managers** liveness. This is part of the [replication strategy](http://www.fogbowcloud.org/rendezvous) used to avoid system crashes.

``` shell
# Known Rendezvous Neighbor aderesses.
neighbors=neighbor1@test.com,neighbor2@test.com
```

The properties below are part the [Result Set Management strategy](http://www.fogbowcloud.org/rendezvous) used to limit, in size, the rendezvous' responses.
``` shell
# Maximum number of managers that should be returned in a whoIsAlive response.
max_whoisalive_manager_count=100

# Maximum number of managers that should be returned in a whoIsAliveSyncresponse.
max_whoisalivesync_manager_count=100

# Maximum number of neighbors that should be returned in a whoIsAliveSyncresponse.
max_whoisalivesync_neighbor_count=100
```

The properties below reference to message "iamalive" sent from Fogbow Manager.
``` shell
# How many time the Fogbow Manager send the message "iamalive" in miliseconds.
iamalive_period=30000
# How many times the Fogbow Rendezvous wait the message "iamalive".
iamalive_max_message_lost=3
```

The property below reference which members are allowed by Fogbow Rendezvous.
``` shell
white_list_class=org.fogbowcloud.rendezvous.core.plugins.whitelist.AcceptAnyWhiteListPlugin
``` 

## Run
To start the rendezvous component, run the start-rendezvous script inside ```./bin```.
``` shell
./bin/start-rendezvous
```
