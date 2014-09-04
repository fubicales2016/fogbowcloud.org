Title: The rendezvous service
url: rendezvous
save_as: rendezvous.html
section: arch
index: 2

# The rendezvous service

The rendezvous service provides a directory for the private clouds that are member of a particular fowbow federation. It allows members to find each other by providing a complete status of the federation. 

## Service architecture

Currently, the rendezvous service provides two methods to the members of a federation: IamAlive and WhoIsAlive.

The first one allows a federation member to report resource information. In this sense, the rendezvous service will periodically receive information from every federation manager, which will also work as a heartbeat, and tag this information with a timestamp. This data is valid for a given expiration interval, which is configurable. After this timeout, data is considered expired and this member will no longer show up as an active member of the federation.

The WhoIsAlive method allows a federation member to ask for the set of active federation members. The rendezvous service will then return resource information about all currently active members.

Also, more than one instance of a rendezvous service can be active to tolerate faults. A rendezvous service instance communicates with other known rendezvous service replicas to synchronize their states. The synchronization is made through a WhoIsAliveSync method. This method sends a message to each of the replicas currently known by a rendezvous service instance asking for the known neighbors, i.e. replicas of the rendezvous service, and the known active members of the federation. The responses received are merged with the information that was previously known.

The rendezvous' response protocols use Result Set Management http://xmpp.org/extensions/xep-0059.html to limit the size of their response. Each user can specify how many items should come in response to a whoIsAlive or whoIsAliveSync call. Each rendezvous has a hard-coded maximum number of items that could come in response, currently, that number is 100. Even if the user defines a maximum number larger or not define a number at all, the response will not be longer than the hard-coded constant. The user can also page fowards, which means asking for a response starting from a specific item. 

All communication is done via XMPP according to the service protocol.

## Service protocol

### IAmAlive

#### Request

```xml 
<iq type="get">
   <query xmlns="http://fogbowcloud.org/rendezvous/iamalive">
      <status>
         <cpu-idle>value</cpu-idle>
         <cpu-inuse>value</cpu-inuse>
         <mem-idle>value</mem-idle>
         <cpu-inuse>value</mem-inuse>
      </status>
   </query>
</iq>
```

#### Response

```xml 
<iq type="result"/>
```
---

### WhoIsAlive

#### Request

```xml 
<iq type="get">
   <query xmlns="http://fogbowcloud.org/rendezvous/whoisalive"/>
</iq>
```

#### Response

```xml 
<iq type="result">
   <query xmlns="http://fogbowcloud.org/rendezvous/whoisalive">
      <item id="federated-cloud-id1">
         <status>
            <cpu-idle>value</cpu-idle>
            <cpu-inuse>value</cpu-inuse>
            <mem-idle>value</mem-idle>
            <cpu-inuse>value</mem-inuse>
            <updated>value</updated>  
         </status>
      </item>
      .
      .
      .
      <item id="federated-cloud-idn">
         <status>
            <cpu-idle>value</cpu-idle>
            <cpu-inuse>value</cpu-inuse>
            <mem-idle>value</mem-idle>
            <cpu-inuse>value</mem-inuse>
            <updated>value</updated>  
         </status>
      </item>
   </query>
</iq>
```

### WhoIsAliveSync

#### Request

```xml 
<iq type="get">
   <query xmlns="http://fogbowcloud.org/rendezvous/synch/whoisalive"/>
</iq>
```

#### Response

```xml 
<iq type="result">
   <query xmlns="http://fogbowcloud.org/rendezvous/synch/whoisalive"/>
      <neighbors>
        <neighbor> id </neighbor>
        .
        .
        .
        <neighbor> id </neighbor>
      </neighbors>
      <managers>
         <manager id = "federated-cloud-id1" >
               <cpu-idle>value</cpu-idle>
               <cpu-inuse>value</cpu-inuse>
               <mem-idle>value</mem-idle>
               <cpu-inuse>value</mem-inuse>
               <updated>value</updated>  
            </status>
         </manager>
         .
         .
         .
         <mananger id="federated-cloud-idn">
            <status>
               <cpu-idle>value</cpu-idle>
               <cpu-inuse>value</cpu-inuse>
               <mem-idle>value</mem-idle>
               <cpu-inuse>value</mem-inuse>
               <updated>value</updated>  
            </status>
         </manager>
      </managers>
   </query>
</iq>
```
### Pagination

#### WhoIsAlive using RSM

##### Request

```xml 
<iq type="get">
   <query xmlns="http://fogbowcloud.org/rendezvous/whoisalive">
      <set xmlns="http://jabber.org/protocol/rsm">
         <max> maximum </max>
         <after> after </after>
      </set>
   </query>
</iq>
```

##### Response

```xml 
iq type="result">
   <query xmlns="http://fogbowcloud.org/rendezvous/whoisalive">
      .
      .
      .
   </query>
   <set>
      <first> first </first>
      <last> last </last>
      <count> count </count>
   </set>
</iq>
```

#### WhoIsAliveSync using RSM
The protocol used is basically the same as the one for WhoIsAlive. As it could be necessary to apply RSM for both managers response items and neighbors response items, the ```<query>``` tag should be followed by ```<neighbors>```  and/or ```<managers>``` tags, and inside those is where the ```<set>``` tag and whatever is inside it should be.



Obs: the updated value is the timestamp of the information; it is formatted according to the ISO 8601 standard. 
