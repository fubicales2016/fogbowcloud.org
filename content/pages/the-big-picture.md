Title: The Big Picture Fogbow Dashboard
url: the-big-picture
save_as: the-big-picture.html
section: install-configure
index: 1

The Big Picture
==========

<center>![The big picture]({filename}/images/the-big-picture.png)</center>

Despite the fact that members of the federation will be trading resources potentially over the Internet, local security policies must be observed. That means that the deployment of the federation components needs to be as little intrusive as possible with regard to user authentication, firewall rules, NAT isolation, demand for public IP addresses, and so on. In particular, a typical deployment of a private cloud should not expose the endpoints for its services – Compute, Storage, Network, Identity etc. Thus, the FM provides a new layer of abstraction that forwards income remote requests for Virtual Machines (VM) instances to the underlying cloud management middleware, keeping these endpoints behind the firewall. In addition to that, FMs must communicate with each other regardless of public IPs or ports exposed through the firewall. In order to address both of these issues, the FM and the FR are implemented as XMPP components that communicate among each other using XMPP servers to route their messages. These servers usually expose port 5269 at the DMZ so that they can reach (and be reached by) any other XMPP server. XMPP components such as the FM and the FR are assigned logical, DNS-resolvable addresses, and message packets exchanged among them are multiplexed through the server-to-server port exposed by their respective XMPP servers.

Another issue that arises from the fact that federation members could be isolated by NATs and firewalls is the lack of connectivity to VM instances from the outside world. Fogbow addresses this issue with a third service, named the Fogbow Reverse Tunnel (FRT). The FRT is responsible for tunnelling service ports from the domain’s intranet to its DMZ. The deployment of this service is mandatory, unless the underlying cloud is able to provide public IP addresses to all VM instances it creates. The FRT can also be used by the VM instances to create other tunnels on demand. For example, a VM instance that needs to provide a service through a web-based interface will have to create a tunnel for the HTPP protocol, so that the users of this service are able to access them from the Internet.
