Title: Plugins
url: install-plugins
save_as: install-plugins.html
section: install
index: 1

# Plugins

The manager component was designed to be agnostic to the underlying cloud technology. There is a plugin layer between this component and the cloud, in a sense that plugins are responsible for translating fogbow requests to what the underlying cloud understands. Plugins are instantiated via reflection, and fully configured via the configuration file. Currently fogbow project make available some plugins, and you can feel free to implement new plugins.

The plugins can be splited into two categories: Identity Plugin and Compute Plugin.

## Idnetity Plugin

The Identity Plugin is responsible for getting and authenticating tokens for local and federation cloud users. The Local Identity Provider and the Federation Identity Provider must be the same when the authentication is done at the Local Cloud Identity Provider. Otherwise, you can configure different plugins to local and federation identities providers. 

### Configure

**OpenStack Idnetity Plugin**

```bash
# jid of your Manager Component
# Example:
xmpp_jid=manager.test.com

```

**OpenNebula Idnetity Plugin** .  

```bash
# jid of your Manager Component
# Example:
xmpp_jid=manager.test.com

```

**OpenStack X509 Plugin** .  

```bash
# jid of your Manager Component
# Example:
xmpp_jid=manager.test.com

```

**OpenStack VOMs Plugin** .  

```bash
# jid of your Manager Component
# Example:
xmpp_jid=manager.test.com

```

## Compute Plugin

The Compute Plugin is responsible for requesting, getting, and deleting virtual instances at the local cloud. Different plugins can require different information depending on their implementation. 

### Configure
As you can see at the [Manager Install Guide](http://www.fogbowcloud.org/install-manager), after installation move the file ```manager.conf.example``` to ```manager.conf```. You need to add the plugins contents according to choosen plugin:

* **OpenStack Compute Plugin:** Manager and Rendezvous XMPP properties that will be used for the comunication between components.  

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

