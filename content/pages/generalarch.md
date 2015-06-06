Title: General architecture
url: architecture
save_as: architecture.html
section: arch
index: 1

# General architecture

The architecture of a fogbow federation of private clouds is based on two main components: the membership manager, called the Fogbow Rendezvous (FR) and the allocation manager, called the Fogbow Manager (FM). The federation of a private cloud requires the implementation and deployment of one instance of the FM component for each private cloud that is to be federated. This component may be configured to interact either with a FR that runs at its local site, or with one that runs at a remote site.

<center>![General architecture]({filename}/images/general.png)</center>

## Fogbow Rendezvous

The FR is in charge of finding out which members of the federation are currently active. It implements a gossip-style lazy replication communication protocol to discover the addresses of other FRs belonging to the same federation, as well as the addresses and estimated capacity of their associated FMs. FRs exchange periodic information among them in order to keep their membership information updated. From time to time, the FM running at a given site interacts with its associated FR to provide its current available idle capacity and to retrieve a list of other currently active FMs, together with an estimation of the resources that they can make available to the federation.

## Fogbow Manager

The distributed and complex nature of a federation of private clouds and the number of cloud management middleware available in the market makes interoperability a big challenge. Also, the more autonomous the members of the federation are, the larger the federation can be, since less restrictions are imposed on members for joining it. The architecture of the FM broadens the range of compatible IaaS technologies supported by concealing communication with the various services provided by the private underlying cloud into implementations of well-defined interoperability plugins. In fact, the plugin concept plays a major role in the architecture and design of the FM, allowing for the easy extensibility of the federation middleware implementation. Not only the communication with the underlying private cloud is defined by plugins, but also key parts of the business logic in the FM, such as authentication, authorization, request prioritization, selection of remote members for request outsourcing, and so on. These behavioural plugins ultimately define the functioning of the FM at each member of the federation. When configuring the FM, the system administrator informs what are the required plugin needed to interact with a particular private cloud deployment.

<center>![Architecture of the Fogbow Manager]({filename}/images/FM.png)</center>

The functionalities of the FM are exposed to its clients via a REST-ful API that implements <a href=http://occi-wg.org/ target="_blank">OCCI</a> extensions. This API can be used to manage asynchronous requests for VMs, as well as functionalities that allow querying the federation members. All other functionalities made available to the local users of the underlying cloud are only available through the native API exposed by the underlying cloud (eg. OpenStack, OpenNebula, OCCI etc.).

## Dealing with firewalls and NATs

Despite the fact that members of the federation will be trading resources potentially over the Internet, local security policies must be observed. That means that the deployment of the federation components needs to be as little intrusive as possible with regard to user authentication, firewall rules, NAT isolation, demand for public IP addresses, and so on. In particular, a typical deployment of a private cloud should not expose the endpoints for its services – Compute, Storage, Network, Identity etc. Thus, the FM provides a new layer of abstraction that forwards income remote requests for Virtual Machines (VM) instances to the underlying cloud management middleware, keeping these endpoints behind the firewall. In addition to that, FMs must communicate with each other regardless of public IPs or ports exposed through the firewall. In order to address both of these issues, the FM and the FR are implemented as <a href=http://xmpp.org/ target="_blank">XMPP</a> components that communicate among each other using XMPP servers to route their messages. These servers usually expose port 5269 at the DMZ so that they can reach (and be reached by) any other XMPP server. XMPP components such as the FM and the FR are assigned logical, DNS-resolvable addresses, and message packets exchanged among them are multiplexed through the server-to-server port exposed by their respective XMPP servers.

<center>![Communication protocol]({filename}/images/communication.png)</center>

Another issue that arises from the fact that federation members could be isolated by NATs and firewalls is the lack of connectivity to VM instances from the outside world. Fogbow addresses this issue with a third service, named the Fogbow Reverse Tunnel (FRT). The FRT is responsible for tunnelling service ports from the domain’s intranet to its DMZ. The deployment of this service is mandatory, unless the underlying cloud is able to provide public IP addresses to all VM instances it creates. The FRT can also be used by the VM instances to create other tunnels on demand. For example, a VM instance that needs to provide a service through a web-based interface will have to create a tunnel for the HTPP protocol, so that the users of this service are able to access them from the Internet.

<center>![Dealing with remote connections]({filename}/images/FRT.png)</center>
