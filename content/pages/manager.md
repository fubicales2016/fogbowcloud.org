Title: Manager
url: manager
save_as: manager.html
section: arch
index: 2

# Manager

The Manager is the federation service that should run on each federation member. It provides an extended OCCI API for final users and interacts with Rendezvous and other Managers. The final user requests resources through extended OCCI API and specifying the amount of them. The requested resources are treated individually (e.g., if a user requests 3 instances, the manager translate it to 3 single requests of 1 instance). 

While there are available resources, the manager create instances on local cloud. Thereafter the it uses information (got from Rendezvous) to choose another manager and submit remaining requests to a remote cloud. This process concludes when the user's requests were fulfilled or they were not into valid period anymore. 

## Manager Architecture

Speak about monitoring Instance; updating token; and submiting open requests.


This component was designed to not depend on a specific cloud technology. There are a Plugin Layer between manager component and the cloud, in this layer fogbow requests are translated to specific cloud request format. 

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
