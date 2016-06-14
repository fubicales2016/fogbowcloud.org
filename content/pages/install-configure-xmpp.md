Title: Install and configure XMPP
url: install-configure-xmpp
save_as: install-configure-xmpp.html
section: install-configure
index: 2

Install and configure XMPP
==========
Extensible Messaging and Presence Protocol (XMPP) is a communications protocol for message-oriented middleware based on XML (Extensible Markup Language). It enables the near-real-time exchange of structured yet extensible data between any two or more network entities.

## Install
We recomend use the [prosody](http://prosody.im/) with server XMPP. For install:
``` shell
$ apt-get update
$ apt-get install prosody
```

## Configure
Access the file configuration prosody.cfg.lua(/etc/prosody/prosody.cfg.lua) and add the components that you need.
 
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
