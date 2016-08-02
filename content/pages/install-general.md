Title: Get started
url: install-general
save_as: install-general.html
section: install-nopage
index: 0

# Get started

Fogbow can be used in several ways. Learn how to install it in the way that will best suit your needs.

## I want to join an existing fogbow federation

If you have a private cloud running, and you want to be part of an existing federation, you need to install a fogbow manager and configure it properly, so that it is able to communicate with your existing cloud, and expose your cloud to the federation. You can learn how to install and configure a fogbow manager [here](http://www.fogbowcloud.org/install-manager).

You will need to configure the fogbow manager to point to one of the rendezvous components in the existing federation. Optionally, you can install a rendezvous component in your site and configure it to snchronize with the other components in the federation. You can learn how to install a fogbow rendezvous instance [here](http://www.fogbowcloud.org/install-rendezvous).

## I want to deploy a fogbow federation myself

If you want to install a new federation, you need to install an instance of the fogbow manager component for every private cloud that you will have in the federation. You can learn how to install and configure a fogbow manager [here](http://www.fogbowcloud.org/install-manager).

Additionally, you need to install rendezvous components to implement the membership management in your federation. You can install as few as one of such components, and point all fogbow managers to this rendezvous, up to as many as one rendezvous component per cloud, and point each fogbow manager to its correspondent rendezvous component. When you deploy more than one rendezvous component they need to be configured to synchronize with each other. You can learn how to install a fogbow rendezvous instance [here](http://www.fogbowcloud.org/install-rendezvous).

## I want to deploy a private cloud using shared resources opportunistically

If you have a private cloud running Openstack, therefore running nova-compute in your compute nodes, and you want to add to this cloud shared desktops in an oportunistic way, so that they show up in the cloud when idle, you can follow the instructions [here](http://www.fogbowcloud.org/install-opportunism).

If you don't have a private cloud already in place, then refer to the Openstack documentation to install the Openstack software in a server, and then follow the steps indicated [here](http://www.fogbowcloud.org/install-opportunism) to deploy the nova-compute in your desktops.
