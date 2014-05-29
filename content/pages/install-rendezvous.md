Title: Rendezvous
url: install-rendezvous
save_as: install-rendezvous.html
section: install
index: 2

# Rendezvous

The Rendezvous component act as a discovery service for a fowbow federated cloud. It allows members to find each other by providing a complete status of the federation.

## Install

As the Rendezvous runs as an XMPP component, you need an XMPP server running and properly configured.
We recommend [prosody](https://prosody.im/) as for its ease of configuration.

If you are using Prosody, you can add a component to its configuration with:
``` shell
Component "rendezvous.test.com"
       component_secret = "password"
```

With an XMPP server already installed and configured, first, get the latest code of the project.
``` shell
git clone https://github.com/fogbow/fogbow-redezvous.git
```
Then, install it
``` shell
mvn install
```

## Configure
After the installation the user can configure a few properties listed below:
``` shell
# jid of your Rendezvous Component.
# Example:
xmpp_jid=rendezvous.test.com

# Component password.
# Example:
xmpp_password=password

# IP address.
# Example:
xmpp_host=127.0.0.1

# Port in which the server will be listening.
# Example:
xmpp_port=5347

# Maximum Interval of time that the Rendezvous Component will keep a cloud in its roster without a sign of activeness.
# Example:
site_expiration=10000
```
## Run
To start Rendezvous, run the start-redezvous script in the bin directory.
``` shell
./bin/start-rendezvous
```
