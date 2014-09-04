Title: Plugins
url: install-plugins
save_as: install-plugins.html
section: install
index: 1

# Plugins

The manager is the fogbow's component that runs in each federation member. It provides a OCCI API for end users and interacts with the rendezvous and other managers. 

This tutorial assumes you have Openstack's OCCI API enabled in your underlying cloud setup. Please read the [Openstack's OCCI guide](https://wiki.openstack.org/wiki/Occi#How_to_use_the_OCCI_interface) to know more.

Also, the manager needs a user registered in the underlying private cloud in order to proxy remote requests to its local resources. For Openstack, you can add new users and projects as is described in the [Openstack Ops guide](http://docs.openstack.org/trunk/openstack-ops/content/projects_users.html#create_new_users). The configuration section of this page explains it in more detail.

## Install
To set up a manager instance, first, get the latest code from github.
```bash
git clone https://github.com/fogbow/fogbow-manager.git
```
Then, install it with Maven
```bash
mvn install
```

## Configure
As you can see at manager instal page, after installation move the file ```manager.conf.example``` to ```manager.conf``` and add the plugins contents according to choosen plugin:

* **XMPP properties:** Manager and Rendezvous XMPP properties that will be used for the comunication between components.  

```bash
# jid of your Manager Component
# Example:
xmpp_jid=manager.test.com

# Component password
# Example:
xmpp_password=password

# IP address.
# Example:
xmpp_host=127.0.0.1

# Port in which the server will be listening.
# Example:
xmpp_port=5347

# jid of your Rendezvous Component
# Example:
rendezvous_jid=rendezvous.test.com
```

