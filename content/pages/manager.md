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

The end-user can interact with Manager to do follow commands:

* Get federation members list;
* Get avaiable resources;
* Get a token access id;
* Request instances;
* Get requests list;
* Get details of a single request;
* Delete a single request;
* Delete all requests;
* Get instances list;
* Get details of a single instance;
* Delete a single instance;
* Delete all instances.

We have a [Manager Client](http://www.fogbowcloud.org/manager-cli) application to help user interaction. Please, check the documentation for more information.

The managers interact each other to do follow actions:

* Request remote instance;
* Get remote instance;
* Remove remote instance.

You can see the [Manager Protocol](#manager-protocol) documentation for more information.

A Manager also can interact with Rendezvous component. The interactions between these components are to:

* Send alive mensage to rendezvous component;
* Know all alive federation members.

More details can be seen at [Rendezvous Protocol](http://www.fogbowcloud.org/rendezvous) documentation.

When a end-user requests resoures, she/he can specify the request type. The manager provides two request types: **one-time** and **persistent**. By default, it is one-time. One-time request can only be fulfilled once, and a persistent request is considered for fulfillment whenever there is no instance running for the same request and the request is into the validity period. If the user does not set validity period, the request is valid from now until forever.

Once a end-user makes a request, the possible request states are: 

* **Open:** The request is not fulfilled;
* **Failed:** The request failed because bad parameters were specified;
* **Fulfilled:** The request is currently active (fulfilled) and has an associated instance;
* **Deleted:** The request was deleted, but it still has an associated instance;
* **Closed:** The request either completed (a Instance was launched and subsequently was interrupted or terminated), or was not fulfilled within the period specified.

All communication among Managers is done via XMPP, according to the Manager Protocol.

## Manager Protocol

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
