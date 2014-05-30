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

Basicaly, the Manager is responsible for three diferent services:

* **Submitting open requests:** it will submit all open requests periodically. It is possible that some user's requests were not have been fulfilled at previous submition, then the manager try to fulfill them periodically. The scheduler interval is configurable at manager configuration file;
* **Monitoring instances:** it monitors request's instances. In this way, the manager identifies when a request's instance does not exist anymore and it updates request state according to request's attributes (possible request states can be seen here). The instance monitoring period is configurable at manager configuration file;
* **Reissuing requests' tokens:** it updates request's token when it is needed. When a request was done by local cloud user, it knows the used access token. However this access token can expire before valid_until attribute or request be fulfilled. In this way, the manager updates the request's token when it is necessary. The token update period is configurable at manager configuration file.

All communication is done via XMPP according to the Manager Protocol.

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
