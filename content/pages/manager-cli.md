Title: Manager cli
url: manager-cli
save_as: manager-cli.html
section: usage
index: 0

Manager command line interface
==========

Manager command line interface is the service for the client that should be use to execute functions the system. It allows user know about federation members, resources provided by fogbow and get, create and delete requests and get and delete instances. 

### Summary
* Member Operations
* Resources Operations
* Token Operations
* Request Operations
* Instance Operations

### 1 - Member Operations. 
Return all federations members.

#### 1.1 - List Federation members 
Get all federation members.
* member (Required).
* --url (Required) : Endpoint.
* --get (Required).

Example :
```bash
<< fogbow-cli member --get --url http://url.com:10000

>> id=ferationid;cpuIdle=20;cpuInUse=10;memIdle=39;memInUse=20;flavor:'small, capacity="1"';
```

### 2 - Resources Operations
Return all resources provided by Fogbow.

#### 2.1 - Get all Fogbow Resources 
Get all resources provided by Fogbow.
* resource (Required).
* --url (Required) : Endpoint.
* --get (Required).

Example :
```bash
<< fogbow-cli resource --get --url http://url.com:10000

>> Category: fogbow-request; scheme="http://schemas.fogbowcloud.org/request#"; class="kind"; title="Request new Instances"; location="http://localhost:8182/request"; attributes="org.fogbowcloud.request.instance-count org.fogbowcloud.request.type org.fogbowcloud.request.valid-until org.fogbowcloud.request.valid-from"
>> Category: fogbow-large; scheme="http://schemas.fogbowcloud.org/template/resource#"; class="mixin"; title="Large Flavor"; location="http://localhost:8182/large"
>> Category: fogbow-linux-x86; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin"; title="Linux-x86 Image"; location="http://localhost:8182/fogbow-linux-x86"
```

### 3 – Token Operation
Return a new token user.

#### 3.1 - Get a new Token
Get a new Token.
* token (Required) : Token user.
* --url (Required) : Endpoint.
* --get (Required).
* --password (Optional) : Password user.
* --username (Required) : Username user.
* --tenantName (Required) .

Example :
```bash
<< fogbow-cli token --get --password mypassword --username myusername --tenantName mytenantname --url http://url.com:10000

>> MIINXgYJKoZIhvcNAQcCoIINTzCCDUsCAQExCTAHBgUrDgMCGjCCC7QGCSqGSIb3DQEHAaCCC6UEgguheyJhY2Nlc3MiOiB7InRva2VuIjogeyJpc3N1ZWRfYXQiOiAiMjAxNC0wNS0
```

### 4 – Request Operations 
Get, create and delete requests.

#### 4.1 – Get request 

##### 4.1.1 – Get all requests
Get all requests user.
* request (Required).
* --url (Required) : Endpoint.
* --get (Required).
* --auth-token (Required) : Token user.

Example :
```bash
<< fogbow-cli request --get --auth-token mytoken --url http://url.com:10000

>> X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
>>  X-OCCI-Location: http://localhost:8182/request/fd745806-4909-4a39-8380-13183b1f197c
```
##### 4.1.2 – Get specific request
Get specific request user.
* request (Required).
* --url (Required) : Endpoint.
* --get (Required).
* --auth-token (Required) : Token user.
* --id (Required) : Request id.

Example :
```bash
<< fogbow-cli request --get --auth-token mytoken --id requestid --url http://url.com:10000

>> RequestId=47536d31-0674-4278-ad05-eff5fdd07257; State=open; InstanceId=232135435-5435345-435345435-43545
```
#### 4.2 - Create requests 
Create requests.
* request (Required).
* --url (Required) : Endpoint.
* --create (Required).
* --auth-token (Required) : Token user.
* --n (Optional) : Number of request.
* --image (Optional) : Image Fogbow.
* --flavor (Optional) : Flavor Fogbow.

Example :
```bash
<< fogbow-cli request --create --n 2 --image myimage --flavor  myflavor --url http://url.com:10000

>> X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
>> X-OCCI-Location: http://localhost:8182/request/fd745806-4909-4a39-8380-13183b1f197c
```
##### 4.2.1 Values Default 
If the user does not fill the optional fields, the default values will be used.
- n = 1.
- image = fogbow-linux-x86.
- flavor = fogbow-small.

Example :
```bash
<< fogbow-cli request --create --url http://url.com:10000  

>> X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
```
#### 4.3 - Delete requests 

##### 4.3.1 – Delete all requests
Delete all requests user.
* request (Required).
* --url (Required) : Endpoint.
* --delete (Required).
* --auth-token (Required) : Token user.

Example :
```bash
<< fogbow-cli request --delete --auth-token mytoken --url http://url.com:10000

>> Ok
```

##### 4.3.2 - Delete specific requests
Delete specific request user.
* request (Required).
* --url (Required) : Endpoint
* --delete (Required).
* --auth-token (Required) : Token user.
* --id (Required) : Request id.

Example :
```bash
<< fogbow-cli request --delete --auth-token mytoken --id requestid --url http://url.com:10000

>> Ok
```

### 5 - Instance Operations
Get and Delete instances.

####5.1 - Get instance

#####5.1.1 -  Get all instance
Get all instances user.
* instance (Required).
* --url (Required) : Endpoint.
* --get  (Required).
* --auth-token  (Required) : Token user.

Example :
```bash
<< fogbow-cli instance --get --auth-token  mytoken --url http://url.com:10000

>> X-OCCI-Location: 3I235356-3432434-324324-3243242f
>> X-OCCI-Location: 4B869582-8907667-123457-0765345c
```
#####5.1.2 -  Get specific instance
Get specific instance user.
* instance (Required).
* --url (Required) : Endpoint.
* --get  (Required).
* --auth-token  (Required) : Token user.
* --id (Required) : Instance id.

Example : 
```bash
<< fogbow-cli instance --get --auth-token mytoken --id instanceid --url http://url.com:10000

>> Category: compute; scheme="http://compute"; class="kind"; title="title"; rel="rel"; location="location"
>> Category: compute; scheme="http://compute"; class="kind"; title="title"; rel="rel"; location="location"
>> Link: </network/admin>; org.openstack.compute.console.vnc="N/A"; occi.compute.architecture="x86"; occi.compute.speed="0.0"
>> X-OCCI-Attribute: org.openstack.compute.console.vnc="N/A"
>> X-OCCI-Attribute: occi.compute.architecture="x86"
>> X-OCCI-Attribute: occi.compute.speed="0.0"
```

####5.2 - Delete instance

#####5.2.1 Delete all instances
Delete all instances user.
* instance (Required).
* --url (Required) : Endpoint.
* --delete  (Required).
* --auth-token  (Required) : Token user.

```bash
<< fogbow-cli instance --delete --auth-token mytoken --url http://url.com:10000

>> Ok
```
#####5.2.2 Delete specific instance
Delete specific instance user.
* instance (Required).
* --url (Required) : Endpoint.
* --delete  (Required).
* --auth-token  (Required) : Token user.
* --id (Required) : Instance id.

```bash
<< fogbow-cli instance --delete --auth-token mytoken --id instanceid --url http://url.com:10000

>> Ok
```
