Title: The Rendezvous Service
url: rendezvous
save_as: rendezvous.html
section: arch
index: 1

# The Rendezvous Service

The Rendezvous Service provides a directory for the private clouds that are member of a particular fowbow federation. It allows members to find each other by providing a complete status of the federation. 

## Service architecture

Currently, the Rendezvous Service provides two methods to the members of a federation: IamAlive and WhoIsAlive.

The first one allows a federation member to report resource information. In this sense, the Rendezvous Service will periodically receive information from every federation manager, which will also work as a heartbeat, and tag this information with a timestamp. This data is valid for a given expiration interval, which is configurable. After this timeout, data is considered expired and this member will no longer show up as an active member of the federation.

The WhoIsAlive method allows a federation member to ask for the set of active federation members. The Rendezvous Service will then return resource information about all currently active members.

Also, a Rendezvous Service instance can communicate with other Rendezvous Service replicas to synchronize, and provide fault tolerance. The synchronization is made through a WhoIsAliveSync method. This method sends a message to each of the replicas currently known by a Rendezvous Service instance asking for the known neighbors, i.e. replicas of the Rendezvous Service, and the known active members of the federation. The responses received are merged with the information that was previously known.

All communication is done via XMPP according to the Rendezvous protocol.

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

Obs: The updated value is the timestamp of information. It will be formatted according to ISO 8601 standard. 
