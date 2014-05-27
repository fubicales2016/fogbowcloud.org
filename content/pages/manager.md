Title: Manager
url: manager
save_as: manager.html
section: arch
index: 2

# Manager

The Manager is the federation service that should run on each federation member. It provides an extended OCCI API for final users and interacts with Rendezvous and other Managers. The final user requests resources through extended OCCI API and specifying the amount of them. The requested resources are treated individually (e.g., if an user requests 3 instances, the manager translates it to 3 single requests of 1 instance). 

While there are available resources, the manager creates instances on local cloud. Thereafter it uses information (got from Rendezvous) to choose another manager and submits remaining requests to a remote cloud. This process concludes when the user's requests were fulfilled or they were not into valid period anymore. 

## Manager Architecture

The Manager component was designed to not depend on a specific cloud technology. There is a Plugin Layer between this component and the cloud, in this layer fogbow requests are translated to specific cloud request format. The plugins that should be used by manager are configurable at manager configuration file. Currently, fogbow provides an implementation to OpenStack cloud.

Basicaly, the Manager is responsable for three diferent services:

* Submiting open requests: it will submit all open requests periodically. It is possible that some user's requests were not have been fulfilled at previous submition, then the manager try to fulfill them periodically. The scheduler interval is configurable at manager configuration file;
* Monitoring of instances: it monitors request's instances. In this way, the manager identifies when a request's instance does not exist anymore and it updates request state according to request's attributes (possible request states can be seen here). The instance monitoring period is configurable at manager configuration file;
* Updating of requests's token: it updates request's token when it is needed. When a request was done by local cloud user, it knows the used access token. However this access token can expire before valid_until attribute or request be fulfilled. In this way, the manager updates the request's token when it is necessary. The token update period is configurable at manager configuration file.

All communication is done via XMPP according to the Manager Protocol.

## Manager Protocol

### Example

#### Request

```xml 
<iq type="get">
   <query xmlns="http://fogbowcloud.org/rendezvous/iamalive">
      <status>
         <cpu-idle>value</cpu-idle>
         <cpu-inuse>value</cpu-inuse>
         <mem-idle>value</mem-idle>
         <cpu-inuse>value</mem-inuse>
      </status>
   </query>
</iq>
```
