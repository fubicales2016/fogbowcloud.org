Title: Install and configure XMPP
url: install-configure-xmpp
save_as: install-configure-xmpp.html
section: install-configure
index: 2

Install and configure XMPP
==========
The Extensible Messaging and Presence Protocol (XMPP) is a protocol for message-oriented communication based on XML (Extensible Markup Language).

TODO: seria bacana uma frase que explicasse pq usamos xmpp no fogbow? nao tenho certe

## Install
We recommend the installation of the [prosody](http://prosody.im/) XMPP server. To install it, run the following commands:
``` shell
$ apt-get update
$ apt-get install prosody
```

## Configure

TODO: what is a component?

For each new **Fogbow Manager** and **Fogbow Rendezvous** installed, it is necessary to add a new component to the `/etc/prosody/prosody.cfg.lua` configuration file. Also, the **Fogbow Manager** and **Fogbow Rendezvous** **xmpp_jid** and 
**xmpp_password** properties, specified in the [Configure Manager](http://www.fogbowcloud.org/install-configure-fogbow-manager#configure), should be used as the **component name** and **component_secret**, as shown below.

```bash
# Manager component
Component "manager.test.com"
        component_secret = "password"
        
# Rendezvous component
# Required when your deployment use a Rendezvous own
Component "rendezvous.test.com"
        component_secret = "password"
```

## Run
After editing the prosody configuration, run below command to restart it.
``` shell
$ service prosody restart
```
