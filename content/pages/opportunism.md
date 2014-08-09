Title: Opportunism
url: opportunism
save_as: opportunism.html
section: arch
index: 3

Opportunism
==========
Opportunism is a feature that allows shared resources to be used by the cloud. Each shared resource can be configured to define when it is deemed usable by the cloud, eg. when it is idle. When the resources are in a state that allow them to be used by the cloud, then they show up to the cloud manager, otherwise they are removed from the cloud, in which case, any virtual machine that is still running in the resource is either suspended or simply terminated.

In the current implementation it is possible to create opportunistic private clouds managed by OpenStack that use desktops in a LAN as computing nodes. Resources are deemed usable by the cloud when there is no mouse/keyboard activity for a configurable period of time. A fogbow manager can be used to federate this opportunitic cloud with other private clouds that use shared resources in an opportunistic way, or dedicated resources in the conventional way.

## Implementation
### Monitoring
The monitoring is implemented using PowerNap, a configurable deamon that executes some actions when the system is idle, and the idleness is determined by monitors that can be configured to watch CPU load, processes, mouse/keyboard, etc.

In the default version, PowerNap allows to customize an action script to determine the action to be executed when the system is idle, but doesn't have a way to define the action executed when the system is back in activity.

The PowerNap version used in this project was customized to allow for custom recover action script, a new property named RECOVER_ACTION was added to the configuration file where the user can set the path to the action script, and the module powernap.py was modified to load the value of this property.

Despite those changes, powernap by default only triggers the recovery action when running in powersave mode (ACTION_METHOD=0), so the PowerNap daemon was changed to also use the recovery action in the best effort mode (ACTION_METHOD=4), that allows for custom actions.

PowerNap has some monitors to check if the host is idle, but the monitor that checks for input devices (mouse and keyboard) just watches USB devices, so a new monitor named PS2Monitor was added to also check PS2 devices.

With those changes, PowerNap is able to be used in the opportunistic component.

### Action
The action part is implemented by two scripts (start-node and stop-node), one for starting the compute node and another for stopping it. Both scripts uses the Python client for NOVA API to enable/disable specific services in order to start/stop the compute node.

The NOVA client interacts with the compute API and allows for service and VMs management. The start-node action script just enables network and compute services. The stop-node action script looks for active VMs, stops them and disables the network and compute services.
