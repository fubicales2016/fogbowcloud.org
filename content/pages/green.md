Title: Greenness
url: green
save_as: green.html
section: arch
index: 5

##What is the Fogbow Green Sitter and why is it necessary?

The Fogbow Green Sitter (FGS) is a tool that aims at helping fogbow deployments to save energy by consolidating applications in active hosts and by shutting down idle hosts with no instances. This service is particularly targeted to opportunistic clouds, since these use resources that would normally be shut down or placed in a low energy consumption mode if they were not being used in a private cloud. Therefore, it is important that these machines are hibernated when they are not use neither by their users, nor by the private cloud. 

##How does the Fogbow Green Sitter work?

In order to decide which host should be powered on/off, it is necessary to get some information from the cloud, such as how many hosts are in the system and how much CPU is available on each host. A **cloud informartion plugin** is responsible to return a list containing those and other information about each host. The FGS was designed to be agnostic to the underlying cloud technology, in that sense, every technology should provide its own cloud information plugin implementation. At the moment, there is a single plugin implementation for Openstack, since we only support opportunistic clouds running Openstack. 

Furthermore, several criteria for deciding when a host should be powered on/off can be implemented, since our decision maker core is also a plugin. The FGS has a default implementation that works for most of the situations that fogbow is deployed. It considers that a host is idle when there is no instances, and there is no cloud computing agent enabled and running in the host, what means that neither the private cloud nor a user is using that host. Then the FGS waits for a grace period before suspending the host, so that we don’t shutdown and turn off computers in such a high frequency that could increase the energy bill instead of lowering its costs due to the powering on/off energy overhead.

The FGS also wakes up sleeping hosts, if there is any with the minimum requirements that fits an incoming fogbow request that cannot be fulfilled with the currently available resources. The FGS will always wake up the host with the largest capacity (considering CPU and RAM), since the most powerful host will potentially be able to create more VMs.

##Architecture

There are two main parts of the FGS: the Agent and the Server. The Agent is the simplest one, it must be installed on each host of the system and will send to the Server the IP of the computer that it is installed and will also receive commands for turning on/off the computer. 

The Server is a little bit more complex. There are three main parts to the Server system: The Cloud Information Plugin, already described, the Green Strategy, which is the heart of the FGS and where decisions are made (who is going to be turned up/down), and the Communication Component that implements the communication with Agents and with the Fogbow Manager. The Communication Component receives the information sent by the agent and sends turn on/off signals to te agent when necessary by using either <a href=http://xmpp.org/ target="_blank">wakeOnLan</a> or <a href=http://en.wikipedia.org/wiki/Wake-on-LAN target="_blank">XMPP</a> protocols. It also receives requests from the Fogbow Manager when there is no resources available in the private cloud, so it powers up a computer when possible.

The following image illustrates the architecture described:
<center>![General architecture of the FGS]({filename}/images/greenSitter.png)</center>
