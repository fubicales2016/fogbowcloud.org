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

The powernap version used in this project was customized to allow the use of a custom recover action script, a new property named RECOVER_ACTION was added to the config file where the user can set the path to the action script and the module powernap.py was modified to load the value of this property.

But, powernap by default only triggers the recover action when running in powersave mode (ACTION_METHOD=0), so the powernap deamon was changed to use also the recover action in the best efford mode (ACTION_METHOD=4), that allows the use of custom actions.

The powernap has some monitors to check if the resource is idle, but the monitor that checks for input devices (mouse and keyboard) just watch USB devices, so a new monitor named PS2Monitor was added to check also PS2 devices.

With this changes, the powernap is able to use in the fogbow opportunism module.

### Action
The action is divided in two scripts (start-node and stop-node), one for start the compute node and other to stop, both scripts uses the Python client for NOVA API to enable/disable specific services in order to start/stop the compute node.

The NOVA client interact with the compute API and allows to manage services and VMs. The start-node action script just creates a new Nova Client and enable network and compute services. The stop-node action script looks for active VMs, stops  them and disables the network and compute services.
