Title: Rendezvous
url: install-rendezvous
save_as: install-rendezvous.html
section: install
index: 2

# Rendezvous

The Rendezvous is the discovery service of the fowbow cloud. It allows members to find each other by providing a complete status of the federation. 

## Install
To set up Rendezvous, if you have an XMPP server alredy installed and configured in your machine, first, get the latest code of the project.
``` shell
git clone https://github.com/fogbow/fogbow-redezvous.git
```
Then, install it in your machine.
``` shell
mvn install -D
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
