Title: Install and configure XMPP
url: install-configure-xmpp
save_as: install-configure-xmpp.html
section: install-configure
index: 2

Install and configure XMPP
==========
The Extensible Messaging and Presence Protocol (XMPP) is a protocol for message-oriented communication based on XML (Extensible Markup Language). In the Fogbow we have two XMPP server components: the **Fogbow Manager** and the **Fogbow Rendezvous**. A XMPP server components is a software module capable of communication with a XMPP server over a protocol.

## Install
Fogbow XMPP components are not tied to any particular server implementation. For the sake of simplicity, this document covers the installation and use of the [prosody](http://prosody.im/) XMPP server which has been used in the Fogbow federations operated by the Fogbow team. To install it, run the following commands:
``` shell
$ apt-get update
$ apt-get install prosody
```

## Configure

For each new **Fogbow Manager** and **Fogbow Rendezvous** installed, it is necessary to add a new component to the `/etc/prosody/prosody.cfg.lua` configuration file. Also, the **Fogbow Manager** and **Fogbow Rendezvous** **xmpp_jid** and 
**xmpp_password** properties, specified in the [Configure Manager](http://www.fogbowcloud.org/install-configure-fogbow-manager#configure), should be used as the **component name** and **component_secret**, as shown below.

```bash
# Manager component
Component "my-manager.internal.mydomain"
        component_secret = "manager_password"
        
# Rendezvous component
Component "my-rendezvous.internal.mydomain"
        component_secret = "rendezvouz_password"
```

## Run
After editing the prosody configuration, run below command to restart it.
``` shell
$ service prosody restart
```
