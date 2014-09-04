Title: Windows Nova Compute
url: install-nova-windows
save_as: install-nova-windows.html
section: install
index: 4

Installation
==========

You will need to install and configure an Windows Nova Compute module in each shared resource (eg. a desktop) that you want to add as a computing node in your private cloud that uses Windows as the OS. Obviously, the resource must be connected to the same LAN used by the computing nodes of your private cloud.

The Windows Nova Compute module has an installer, so all you have to do is download an run the fogbow-nova-compute executable.

Configuration
==========

After the installation you will have to configure the nova.conf file providing information about the cloud management tools used by openstack. Contact your cloud admnistrator about this information

The configuration file is located at the installation directory of the Windows Nova Compute Module

An example of a nova.conf file configuration is shown bellow. the lines marked with the "#=>Necessary" tags are the one with information the needs to pertain to your cloud
```bash
[DEFAULT]

#glance
glance_api_servers=10.1.0.43:9292 #=>Necessary
host=windows-host2 #=>Necessary
#Rabbit configuration
rabbit_host=10.1.0.43 #=>Necessary
rabbit_password=labstack #=>Necessary

#Neutron configuration
service_neutron_metadata_proxy=True
network_api_class=nova.network.neutronv2.api.API
neutron_url=http://10.1.0.43:9696  #=>Necessary
neutron_admin_username=neutron #=>Necessary
neutron_admin_password=labstack #=>Necessary
neutron_admin_tenant_name=service #=>Necessary
neutron_region_name=RegionOne #=>Necessary
neutron_admin_auth_url=http://10.1.0.43:5000/v2.0 #=> Necessary
neutron_auth_strategy=keystone

#security groups
security_group_api=neutron

#logs and debug
debug=true
verbose=true
default_log_levels=amqplib=WARN,sqlalchemy=WARN,boto=WARN,suds=INFO,keystone=INFO,eventlet.wsgi.server=WARN
log_file=C:\images-nova\log

#driver
compute_driver=qemuwin.QemuWinDriver
qemu_home=C:\Program Files\qemu 


#network
qemuwin_vif_driver = nova.virt.qemuwin.vif.LibvirtHybridOVSBridgeDriver
firewall_driver = nova.virt.qemuwin.firewall.IptablesFirewallDriver
instances_path=C:\images-nova\instances

#keystone
auth_strategy=keystone

#Metadata configuration
nova_metadata_host=10.1.0.43 #=>Necessary
nova_metadata_port=8775 #=>Necessary

[keystone_authtoken]
auth_strategy=keystone
auth_host=10.1.0.43 #=>Necessary
```
