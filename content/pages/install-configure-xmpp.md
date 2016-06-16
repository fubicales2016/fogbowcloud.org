Title: Install and configure XMPP
url: install-configure-xmpp
save_as: install-configure-xmpp.html
section: install-configure
index: 2

Install and configure XMPP
==========
Extensible Messaging and Presence Protocol (XMPP) is a communications protocol for message-oriented middleware based on XML (Extensible Markup Language). It enables the near-real-time exchange of structured yet extensible data between any two or more network entities.

## Install
We recommend the use of [prosody](http://prosody.im/) as XMPP server. In order to install it, run the following commands:
``` shell
$ apt-get update
$ apt-get install prosody
```

## Configure
Edit the configuration file prosody.cfg.lua(/etc/prosody/prosody.cfg.lua) and add the XMPP components that you need.
 
Per example, it is used in the configuration for fogbow manager. You can see in the [Configure Manager](http://www.fogbowcloud.org/install-configure-fogbow-manager#configure).
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
``` shell
$ service prosody start
```
