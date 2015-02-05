Title: Greenness
url: green
save_as: green.html
section: arch
index: 5

Greenness
==========
Server consolidation in IaaS datacenters allows for energy savings by reducing the number of servers that need to remain switched on. Some cloud management middleware have support for this feature. Such support is crucial for large datacenters.

When fogbow is used to deploy an IaaS cloud using shared desktops, the number of nodes can be very high. This is also a setting for which some green IT approach is crucial, since if these desktops were not to be used in the cloud, they would probably be switched off. 

#What is the Fogbow Green Sitter and why is it necessary?

The Fogbow Green Sitter is a tool that aims on helping Fogbow users saving energy by consolidating applications in active hosts and by shutting down idle hosts with no instances. In situations where hosts live in an energy-constrained environment,  it is important that, we implement an automatic policy that turns off hosts after instances created by the cloud middleware (such as Openstack and Open Nebula) in the private cloud infrastructure are finished and no user is using the host (in case of an opportunistic deployment)..

# Implemenatation

We are still developing the features to support green IT. Watch this space.
