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

The information cited bellow should be changed on the file to fit your cloud:
```bash
[DEFAULT]

#The Glance project provides services for discovering, registering, and retrieving virtual
#machine images. 
glance_api_servers=10.1.0.43:9292

#RabbitMQ is the default AMQP server used by many OpenStack services.
rabbit_host=10.1.0.43
rabbit_password=labstack

#Neutron is an OpenStack project to provide "networking as a service" between interface
#devices (e.g., vNICs) managed by other Openstack services (e.g., nova)
neutron_url=http://10.1.0.43:9696  
neutron_admin_username=neutron 
neutron_admin_password=labstack 
neutron_admin_tenant_name=service
neutron_region_name=RegionOne 
neutron_admin_auth_url=http://10.1.0.43:5000/v2.0 

#The Compute service uses a special metadata service to enable virtual machine instances
#to retrieve instance-specific data
nova_metadata_host=10.1.0.43
nova_metadata_port=8775 
auth_host=10.1.0.43 


##Commonly these four services are run on the same machine
```
