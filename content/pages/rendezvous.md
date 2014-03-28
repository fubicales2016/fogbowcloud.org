Title: Rendezvous
url: rendezvous
save_as: rendezvous.html
section: arch
index: 1

# Rendezvous

The Rendezvous is the discovery service of the fowbow cloud. It allows members to find each other by providing a complete status of the federation. 

## Rendezvous architecture

Currently, the Rendezvous provides two methods to federation members: IamAlive and WhoIsAlive.

The first one allows a federation member to report resource information. In this sense, the Rendezvous component will periodically receive information from every federation manager, which will also work as a heartbeat, and tag this information with a timestamp. This data is valid for a given expiration interval, which is configurable. After this timeout, data is considered expired and this member will no longer show up as an active member of the federation.

The WhoIsAlive method allows a federation member to ask for the set of active federation members. The rendezvous component will then return resource information about all currently active members.

All communication is done via XMPP according to the Rendezvous protocol.

## Rendezvous protocol

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
Obs: The updated value is the timestamp of information. It will be formatted according to ISO 8601 standard. 