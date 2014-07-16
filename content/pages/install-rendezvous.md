Title: Rendezvous
url: install-rendezvous
save_as: install-rendezvous.html
section: install
index: 2

# Rendezvous

The Rendezvous component act as a discovery service for a fowbow federation. It allows members to find each other by providing a complete status of the federation.

## Install

As the Rendezvous runs as an XMPP component, you need an XMPP server running and properly configured.
We recommend [prosody](https://prosody.im/) as for its ease of configuration.

If you are using Prosody, you can add a component to its configuration with:
``` shell
Component "rendezvous.test.com"
       component_secret = "password"
```

With an XMPP server already installed and configured, get the latest code of the project.
``` shell
git clone https://github.com/fogbow/fogbow-rendezvous.git
```
Then, install it
``` shell
mvn install
```

## Configure
After the installation, move the file ```rendezvous.conf.example``` to ```rendezvous.conf``` and edit its contents:

``` shell
# XMPP address of your Rendezvous Component, as configured in the XMPP server.
xmpp_jid=rendezvous.test.com

# XMPP component password, as configured in the XMPP server.
xmpp_password=password

# Address of the host running the XMPP component.
xmpp_host=127.0.0.1

# Port in which the XMPP server will be listening for components, as configured in the XMPP server.
xmpp_port=5347

# Maximum amount of time that the Rendezvous Component will keep a member active without a heartbeat.
site_expiration=10000

# Known Rendezvous Neighbor aderesses.
neighbors=neighbor1@test.com,neighbor2@test.com
```
## Run
To start the Rendezvous component, run the start-rendezvous script inside ```./bin```.
``` shell
./bin/start-rendezvous
```
