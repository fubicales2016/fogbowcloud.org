Title: General architecture
url: architecture
save_as: architecture.html
section: arch
index: 1

# General architecture

## Federation overview

A Fogbow Manager is deployed for each private cloud that wants to join the federation. The manager exposes an OCCI API that can be invoked by the local users to request services. Through this API, local users can access resources in the local cloud, as well as in any other cloud that is member of the federation.

The configuration of the Fogbow Manager associates it with a Rendezvous Service that is responsible for letting the different Fogbow Managers in the federation to find each other. The Fogbow Manager uses XMPP to communicate with the Rendezvous Service, informing its status, and collecting status information of all available Fogbow Managers.

When the local resources are not enough to fulfill the local users' requests, the Fogbow Manager tries to get resources from other managers belonging to the federation. This communication also uses XMPP.

![General architecture]({filename}/images/fogbow-arch-general.png)

## Per-member overview

The Fogbow Manager has a Plugin Layer that allows it to easily interact with different cloud management middleware that might be running on the underlying private cloud.

![Detailed architecture]({filename}/images/fogbow-arch-detailed.png)
