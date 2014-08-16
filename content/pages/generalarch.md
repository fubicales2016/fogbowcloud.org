Title: General architecture
url: architecture
save_as: architecture.html
section: arch
index: 1

# General architecture

## Federation overview

A fogbow manager is deployed for each private cloud that wants to join the federation. The manager exposes an OCCI API that can be invoked by the local users to request services. Through this API, local users can access resources in the local cloud, as well as in any other cloud that is member of the federation.

The configuration of the fogbow manager associates it with a rendezvous service that is responsible for letting the different fogbow managers in the federation to find each other. The fogbow manager uses XMPP to communicate with the rendezvous service, informing its status and endpoints, and collecting status and endpoints information related to all available fogbow managers.

When the local resources are not enough to fulfill the local users' requests, the fogbow manager tries to get resources from other managers belonging to the federation. This communication also uses XMPP.

<center>![General architecture]({filename}/images/federation.png)</center>

## Federation member overview

The fogbow manager has a plugin layer that allows it to easily interact with different cloud management middleware that might be running on the underlying private cloud. When configuring the fogbow manager, the system administrator informs what are the required plugin needed to interact with a particular private cloud deployment.

<center>![Detailed architecture]({filename}/images/manager.png)</center>
