Title: Manager
url: manager
save_as: manager.html
section: arch
index: 2

# Manager

The Manager is the fogbow component that runs in each federation member. It provides an extended OCCI API for end users and interacts with the [Rendezvous](http://www.fogbowcloud.org/rendezvous) and other Managers. The end user creates requests for instances via an extended OCCI API by specifying the desired amount of instances. Requests for each instances are then treated individually (e.g., if an user creates requests 3 instances, the manager translates it to 3 single requests of 1 instance). 

While there are available resources, the manager will create instances in the underlying private cloud. Thereafter, it chooses remote members to submit remaining requests. Open requests are rescheduled regularly until they are fulfilled or no longer valid. 

## Manager Architecture

The Manager component was designed to be agnostic to the underlying cloud technology. There is a plugin layer between this component and the cloud, in a sense that plugins are responsible to translate fogbow requests to what the underlying cloud understands.  Plugins are instantiated via reflection, and fully configured via the configuration file. Currently, fogbow provides a set of plugin implementations that is able to interact with an OCCI-enabled Openstack.

Internally, the Manager is responsible for three periodic services:

* **Submitting open requests:** the manager submits all open requests periodically. The rescheduling interval is configurable in the manager configuration file;
* **Monitoring instances:** the manager checks all instances that it is aware of and identifies when an instance is no longer active. Then it updates the state of the corresponding requests according to their type (one-time|persistent). The instance monitoring period is also configurable in the manager configuration file;
* **Reissuing requests' tokens:** the manager updates requests' tokens when it is needed. When a request was done by an user of the underlying cloud, it is aware of the access token being used. However this access token can expire before  request gets fulfilled or before the valid_until attribute. In this way, the manager updates the request's token when it is necessary. The token update period is configurable in the manager configuration file.

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

A Manager will also interact with a Rendezvous component. Those interactions will:

* Send liveness messages;
* List active federation members.

See more details at the [Rendezvous Protocol](http://www.fogbowcloud.org/rendezvous) documentation.

When an end-user requests resoures, s/he is able to specify a request type, which can be **one-time** or **persistent**, default set to **one-time**. An one-time request can only be fulfilled once, whilst a persistent one is considered for scheduling while it has no instance allocated and it is still into the validity period.

Once a end-user makes a request, the possible request states are: 

* **Open:** The request is active but it has no allocated instance;
* **Failed:** The request failed because bad parameters were specified;
* **Fulfilled:** The request is currently active and has an associated instance;
* **Deleted:** The request was deleted, but it still has an associated instance;
* **Closed:** The request either completed (an instance was launched and subsequently was interrupted or terminated), or was not fulfilled within the period specified.

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
