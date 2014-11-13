Title: Opportunism
url: install-opportunism
save_as: install-opportunism.html
section: install
index: 3

Installation
==========

You will need to install and configure an opportunism module in each shared resource (eg. a desktop) that you want to add as a computing node in your private cloud. Obviously, the resource must be connected to the same LAN used by the computing nodes of your private cloud.

Unix
==========

##Installation
###Install from source
The opportunism module uses python's Distutils, so all you need to do is to get the latest code and run the setup.py.

``` bash
git clone https://github.com/fogbow/fogbow-opportunism.git
cd fogbow-opportunism
sudo python setup.py install
```

###Install from debian package
Download the [lastest stable package](http://downloads.fogbowcloud.org/nightly/debian/fogbow-powernap/fogbow-powernap_latest.deb)
```bash
wget http://downloads.fogbowcloud.org/nightly/debian/fogbow-powernap/fogbow-powernap_latest.deb
```
And install it with dpkg
```bash
sudo dpkg -i fogbow-powernap_latest.deb
```

The opportunistic module files will be located at /var/lib/fogbow-opportunism.

##Configuration

After the installation, you can configure which monitors will be used and how much time the powernap will wait before assuming that the host is idle.

The configuration file is located at ```/etc/powernap/config```. A commented configuration file follows on:

```bash
# Number of seconds that all monitors must have no activity or must be absent.
# The default is an absence period of 30 seconds.
# Example:
#   ABSENT_SECONDS = 30
ABSENT_SECONDS = 300

# The powernap daemon will wake every INTERVAL_SECONDS and check for activity
# to all of the monitors.  Lower values of INTERVAL_SECONDS will provide
# more detailed process monitoring but will consume more system resources.
# Higher values of INTERVAL_SECONDS will consume less system resources, but
# might miss activity that execute for less than INTERVAL_SECONDS.
# The minimum runtime of any monitor should not be less than INTERVAL_SECONDS.
# The default interval is 1 second.
# Example:
#   INTERVAL_SECONDS = 1
INTERVAL_SECONDS = 10

# Action to run when the monitor detects activity in the resource
# The default action is "/var/lib/fogbow-opportunism/actions/openstack/stop-node"
RECOVER_ACTION = "/var/lib/fogbow-opportunism/actions/openstack/stop-node"

# Action to run when the resource is idle
ACTION = "/var/lib/fogbow-opportunism/actions/openstack/start-node"
```

Monitors' configuration:

```bash
# The [ConsoleMonitor] section enables or disables  monitoring of activity
# in the Console (tty), also tracking activity from any locally connected
# mouse and keyboard (PS2 Only).
# The default is enabled, set to 'y'. It can be disabled by setting it to 'n'.
# Examples:
#  console = y
#  console = n
[ConsoleMonitor]
ptmx = y

# The [PS2Monitor] section enables or disables monitoring of activity
# in the PS2 input devices (ex.: mouse and keyboard)
[PS2Monitor]
ps2 = y

# The [InputMonitor] section lists the USB Input devices for which to track
# events. Currently, only two types of devices are supported, mouse and keyboard.
# Both InputMonitor's are enabled by default. In the case there are no USB
# devices connected, PowerNap will ignore these settings.
# To disable, set them to "n" or "no", or simply comment them.
# Examples:
#  keyboard = n
#  keyboard = y
[InputMonitor]
keyboard = y
mouse = y

# The [WoLMonitor] section lists all ports on which the WoL Monitor will be
# listening for WoL Packets for any of the network interfaces.
# Once a WoL Packet is received, the WoLMonitor will compare the data received
# with all the network interfaces (eth's) to determine wether it is destined
# for any of the network interfaces.
# The default is to monitor ports 7 and 9 for WoL data packets. It can also be
# set to any other port on which the machine is receiving WoL packets
# Example:
#  wol1052 = 1052
[WoLMonitor]
wol7 = 7
wol9 = 9

# The [ProcessMonitor] section lists all the processes to Monitor by using
# regular expressions.
# Each item listed will be compared against the output of "ps -eo args".
# The default is to monitor /sbin/init, which should always be running.
# Examples:
#  mplayer = "mplayer "
#  sshd = "sshd: .*\[priv\]$"
#  kvm = "kvm "
[ProcessMonitor]
init = "^/sbin/init"

# The [LoadMonitor] section defines the load threshold.  When the system load
# according to /proc/loadavg is above this value, then system will be deemed
# 'active' and will not powernap.  If the system is already powernapping, then
# the system will awake out of the powernap mode if the load raises above the
# threshold.
# If the threshold is set to "n" (which is default), threshold is automatically
# calculated to be the number of online processors, as determined by:
#   getconf _NPROCESSORS_ONLN
# Example:
#   threshold = 1.5
#   threshold = 9999
#   threshold = 0
#   threshold = n
[LoadMonitor]
threshold = n

# The [TCPMonitor] section lists all the TCP ports on which to watch for
# established connections using netstat(8). It supports both, single TCP
# as well as port ranges.
# There is no default TCP Monitor.
# Examples:
#  ssh = 22
#  http = 80
#  https = 443
#  other = 64500-65000
[TCPMonitor]
ssh = 22

# The [UDPMonitor] section lists all the UDP ports on which to listen
# for data.
# Each item listed will BIND a UDP port and listen to any data. Keep
# in mind that this port will be bined and no other application will
# be able to use this port.
# There is no default UDP Monitor.
# Examples:
#  udp-1 = 1025
#  udp-2 = 2048
[UDPMonitor]
udp-1025 = 1025

# The [IOMonitor] section lists all the processes to Monitor for IO
# activity. A regular expressions is used to find the processes PIDs, which
# are later used to monitor IO.
# There is no default IO Monitor.
# Examples:
#  kvm-io = "kvm"
#  mysqld-io = "mysql"
[IOMonitor]
kvm-io = "kvm"
mysqld-io = "mysql"

# The [DiskMonitor] section lists the disk devices for which to track
# standby/sleep status.  If any of the devices are active/idle the
# system will be deemed 'active' and will not powernap. Generally useful
# for monitoring data drives (e.g. NAS), but will not typically work to
# monitor the root drive. Note also that this plugin only reacts to the
# state of the drive and does not modify the behavior of the drive
# directly. Therefore it only makes sense to monitor a drive that has
# already been configured to standby or sleep.  
# To disable checking specific drives, set them to "n" or "no",
# or simply comment them.
# Examples:
#  sda = y
#  sdb = n
[DiskMonitor]
sda = y
```


Windows
==========

##Installation
The opportunism module is included on Windows Nova Compute module, so by installing that you will also get this feature

##Configuration

```bash
[GENERAL]
nova_conf_file = C:\\nova\\nova.conf
log_file = log
#check_interval is the time inbetween two checks for idleness in the system
check_interval = 30 
#idle_interval is the interval of inactive time after which nova should be habilited
idle_interval = 1200
```
