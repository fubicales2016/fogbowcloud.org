Title: Get started
url: install-general
save_as: install-general.html
section: install
index: 0

# Get started

Fogbow can be used in several ways. Learn how to install it in the way that will best suit your needs.

## I want to join an existing fogbow federation

If you have a private cloud running, and you want to be part of an existing federation, you need to install a fogbow manager and configure it properly, so that it is able to communicate with your existing cloud, and expose your cloud to the federation. You can learn how to install and configure a fogbow manager [here](http://www.fogbowcloud.org/install-manager).

## I want to deploy a fogbow federation myself

If you want to install a new federation, you need to install an instance of the rendezvous component, as well as a manager component for every private cloud that you will have in the federation. Then, of course, you will need to point every manager to the same rendezvous service, so that they will find each other. You can learn how to install a fogbow rendezvous instance [here](http://www.fogbowcloud.org/install-rendezvous).

## I want to deploy a private cloud using shared resources opportunistically

If you have a private cloud running Openstack, therefore running nova-compute in your compute nodes, and you want to add to this cloud shared resources in an oportunistic way, in the sense that only idle nodes will show up to the cloud, you can follow the instructions [here](http://www.fogbowcloud.org/install-opportunism).

