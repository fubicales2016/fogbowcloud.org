Title: Fogbow manager
url: manager
save_as: manager.html
section: arch
index: 2

# Fogbow manager

The Manager is the fogbow component that runs in each federation member. It provides an OCCI API for end users and interacts with its associated [Rendezvous Service](http://www.fogbowcloud.org/rendezvous) and other Managers. Fogbow's OCCI implementation provides a unique feature that allows end users to create requests for large amounts of instances, typically beyond what can be provided by the private cloud to which the user has access.

Requests for a number of instances are then treated individually (eg., if a user creates a request for 100 instances, the manager translates it to 100 requests of 1 instance). While there are available resources, the manager will create instances in the underlying private cloud. Thereafter, it chooses remote members to submit the remaining requests. Users can start using the instance created as soon as they become available. Open requests are rescheduled regularly until they are fulfilled or no longer valid.

## Manager architecture

The Manager component was designed to be agnostic to the underlying cloud technology. There is a plugin layer between this component and the cloud, in a sense that plugins are responsible for translating fogbow requests to what the underlying cloud understands. Plugins are instantiated via reflection, and fully configured via the configuration file.

Currently, fogbow provides a set of plugin implementations that is able to interact with an OCCI-enabled Openstack. Plugins for the standard OpenStack API and OpenNebula are under development.

The standard OCCI features are only available if the underlying cloud provides support to them, since the Manager simply forward these calls to the underlying cloud. On the other hand, the new feature that allows users to request large numbers of instances is always supported by fogbow, for all cloud management middleware for which the appropriate plugins are available.

For that, internally, the Manager is responsible for three periodic activities:

* **Submitting open requests:** the manager submits all open requests periodically. The rescheduling interval is configurable in the manager configuration file;
* **Monitoring instances:** the manager checks all instances that it is aware of and identifies when an instance is no longer active. Then it updates the state of the corresponding request according to its type (one-time|persistent). The instance monitoring period is also configurable in the manager configuration file;
* **Reissuing requests' tokens:** the manager updates requests' tokens when it is needed. When a request was done by a user of the underlying cloud, it is aware of the access token being used. However this access token can expire before the request gets fulfilled, or before the valid_until attribute. In this way, the manager updates the request's token when it is necessary. The token update period is configurable in the manager configuration file.

The end-user can interact with a Manager component to perform the following operations:

* List the federation members;
* Get a token access id;
* Request instances;
* List requests;
* Delete requests;
* List instances;
* Delete instances.

We have a fogbow command line interface that helps users to interact with a Manager. Please, check its [documentation](http://www.fogbowcloud.org/manager-cli) for more information.

Managers interact with each other to perform the following:

* Request instances;
* Get detailed information about remote instances;
* Remove remote instances.

You can check the [Manager Protocol](#manager-protocol) for more information.

A Manager will also interact with a Rendezvous Service. Those interactions will:

* Send liveness messages;
* List active federation members.

See more details at the [Rendezvous Protocol](http://www.fogbowcloud.org/rendezvous) documentation.

When an end user requests resoures, s/he is able to specify a request type, which can be **one-time** or **persistent**, default set to **one-time**. A one-time request can only be fulfilled once, whilst a persistent one is considered for scheduling while it has no instance allocated and it is still into the validity period.

Once a end-user makes a request, the possible request states are: 

* **Open:** The request is active but it has no allocated instance;
* **Failed:** The request failed because bad parameters were specified;
* **Fulfilled:** The request is currently active and has an associated instance;
* **Deleted:** The request was deleted, but it still has an associated instance;
* **Closed:** The request either completed (an instance was launched and subsequently was interrupted or terminated), or was not fulfilled within the period specified.

## Authentication/Authorization

A manager is able to allocate Virtual Machines (VMs) on its associated local private cloud using a user of this cloud specially configured for this purpose by the administrator of the local private cloud. In particular, the administrator autonomously defines the usage policies that apply to this user, eg. the quota that is assigned to it. We will refer to this special user as the fogbow user.

A user of the fogbow federation - referred as a federation user - acquires VMs by interacting with a particular fogbow manager. The process of authentication and authorization involves two levels: the Federation Level, in which the fogbow manager will authenticate federation users and authorize them to create requests in the federation, and the Local Level, used by the underlying cloud services to authenticate users (fogbow or federation users). If the user is not authenticated or authorized at the federation level, than it will not be able to request VMs. Otherwise, it is able to request VMs to the fogbow manager that uses the fogbow user to request resources on behalf of the federation user. Additionally, if the federation user is also authenticated and authorized at the local level, it can request additional resources using its local user.

Authorized federation users may request as many VMs as needed, even if this number is well above their local quota. At first, the fogbow manager will try to create VMs locally within the local user quota. If there are not enough resources available or the federation user is not authenticated or authorized in the local cloud, the fogbow manager will use the fogbow user to request VMs. Again, when there is no more resources available, the fogbow manager will contact other federation members to create VMs in remote private clouds. The remote members, in turn, will use their fogbow user to create VMs for remote requests.

There is a third level of authentication and authorization that involves only the fogbow managers, or members. Federation members exchange X.509 certificates when joining a fogbow federation, and those are used in the authentication and authorization process that tells if a member may ask for or donate resources to a remote member, as per Figure X. This procedure, though, is out of the scope of this document.

## SSH Tunneling

To be detailed.

## Manager Protocol

All communication among Managers is done via XMPP, according to the Manager Protocol.

### Request Remote Instance

#### Request

```xml 
<iq type="set">
   <query xmlns="http://fogbowcloud.org/manager/request">
      <category>
         <class>category-class1</class>
         <term>category-term1</term>
         <scheme>category-scheme1</scheme>
      </category>
      ...
      <category>
         <class>category-classN</class>
         <term>category-termN</term>
         <scheme>category-schemeN</scheme>
      </category>
      <attribute>
         <var>att-name1</var>
         <value>att-value1</value>
      </attribute>
      ...
      <attribute>
         <var>att-nameN</var>
         <value>att-valueN</value>
      </attribute>
   </query>
</iq>
```

#### Response

```xml 
<iq type="result">
   <query xmlns="http://fogbowcloud.org/manager/request">
      <instance>
         <id>instance-id</id>
      </instance>
   </query>
</iq>
```

### Get Remote Instance

#### Request

```xml 
<iq type="get">
   <query xmlns="http://fogbowcloud.org/manager/getinstance">
      <instance>
         <id>instance-id</id>
      </instance>
   </query>
```

#### Response

```xml 
<iq type="result">
   <query xmlns="http://fogbowcloud.org/manager/getinstance">
      <instance>
         <id>instance-id</id>
         <link>
            <link>link-name</link>
            <attribute val="att-name1">att-value1</attribute>
            ...
            <attribute val="att-nameN">att-valueN</attribute>
         </link>
         <resource>
            <category>
               <class>category-class1</class>
               <term>category-term1</term>
               <scheme>category-scheme1</scheme>
               <attribute>att-name1</attribute>
               ...
               <attribute>att-nameN</attribute>
               <action>action-name1</acttion>
               ...
               <action>action-nameN</acttion>
            </category>
            <location>value</location>
            <title>value</title>
            <rel>value</rel>
         </resource>
         ...
         <resource>
            <category>
               <class>category-classN</class>
               <term>category-termN</term>
               <scheme>category-schemeN</scheme>
               <attribute>att-name1</attribute>
               ...
               <attribute>att-nameN</attribute>
               <action>action-name1</acttion>
               ...
               <action>action-nameN</acttion>
            </category>
            <location>value</location>
            <title>value</title>
            <rel>value</rel>
         </resource>
         <attribute val="att-name1">att-value1</attribute>
         ...
         <attribute val="att-nameN">att-valueN</attribute>
      </instance>
   </query>
</iq>
```

### Remove Remote Instance

#### Request

```xml 
<iq type="set">
   <query xmlns="http://fogbowcloud.org/manager/removeinstance">
      <instance>
         <id>instance-id</id>
      </instance>
   </query>
```

#### Response

```xml 
<iq type="result"/>
```
