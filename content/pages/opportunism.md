Title: Opportunism
url: opportunism
save_as: opportunism.html
section: arch
index: 3

Opportunism
==========
Opportunism is a feature that allows idle resources to be used by the cloud. Resources being used by a local user shouldn’t show up to the cloud.

## Requirements
* nova-compute should be disabled and its instances killed when there is mouse/keyboard activity.
* it must come back when the host is idle for a certain amount of time.

## Implementation
### Monitoring
The monitoring is implemented using PowerNap that is a configurable deamon that executes some actions when the system is idle, and the idleness is determined by monitors that can be configured to watch CPU load, processes, mouse/keyboard, etc.

In default version, powernap allows to customize an action script to determine the action to be executed when the system is idle, but doesn't have a way to define the action executed when the system is back in activity.

The powernap version used in this project was customized to allow for custom recover action script, a new property named RECOVER_ACTION was added to the configuration file where the user can set the path to the action script, and the module powernap.py was modified to load the value of this property.

Despite those changes, powernap by default only triggers the recovery action when running in powersave mode (ACTION_METHOD=0), so the powernap daemon was changed to alse use the recovery action in the best effort mode (ACTION_METHOD=4), that allows for custom actions.

The powernap has some monitors to check if the host is idle, but the monitor that checks for input devices (mouse and keyboard) just watches USB devices, so a new monitor named PS2Monitor was added to also check PS2 devices.

With those changes, the powernap is able to be used in the opportunistic component.

### Action
The action is divided in two scripts (start-node and stop-node), one for start the compute node and other to stop, both scripts uses the Python client for NOVA API to enable/disable specific services in order to start/stop the compute node.

The NOVA client interacts with the compute API and allows for services management and VMs management. The start-node action script just enables network and compute services. The stop-node action script looks for active VMs, stops  them and disables the network and compute services.
