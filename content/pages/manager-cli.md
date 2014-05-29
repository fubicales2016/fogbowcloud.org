Title: Manager client
url: manager-cli
save_as: manager-cli.html
section: usage
index: 0

Manager command line interface
==========

Manager command line interface is the service used to execute functions in the system. It allows user to get information about federation members, resources provided by fogbow and create requests to manage instances.

### Summary
* Member Operations
* Resources Operations
* Token Operations
* Request Operations
* Instance Operations

### 1 - Member Operations. 

Return all federation members.

#### 1.1 - List Federation members 

Get all federation members.

* member (Required)
* --url (Required) : Endpoint
* --get (Required)

Example :
```bash
$ bin/fogbow-cli member --get --url http://localhost:8182

id=ferationid1;cpuIdle=20;cpuInUse=10;memIdle=39;memInUse=20;flavor:'small, capacity="1"';
id=ferationid2;cpuIdle=30;cpuInUse=20;memIdle=49;memInUse=20;flavor:'large, capacity="2"';
```

### 2 - Resources Operations

Return all resources provided by Fogbow.

#### 2.1 - Get all Fogbow Resources 

Get all resources provided by Fogbow. 

* resource (Required)
* --url (Required) : Endpoint
* --get (Required)

Example :
```bash
$ bin/fogbow-cli resource --get --url http://localhost:8182

Category: fogbow-request; scheme="http://schemas.fogbowcloud.org/request#"; class="kind"; title="Request new Instances"; location="http://localhost:8182/request"; attributes="org.fogbowcloud.request.instance-count org.fogbowcloud.request.type org.fogbowcloud.request.valid-until org.fogbowcloud.request.valid-from"
Category: fogbow-large; scheme="http://schemas.fogbowcloud.org/template/resource#"; class="mixin"; title="Large Flavor"; location="http://localhost:8182/large"
Category: fogbow-linux-x86; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin"; title="Linux-x86 Image"; location="http://localhost:8182/fogbow-linux-x86"
```

### 3 – Token Operation

Return a new user's token.

#### 3.1 - Get a new Token

Get a new Token.

* token (Required) : User's token
* --url (Required) : Endpoint
* --get (Required)
* --password (Optional) 
* --username (Required)
* --tenantName (Required) 

Observation : If the password is not passed in the command line, it will be requested in the console. Password is optional in the command but needed in operation.

Example :
```bash
$ bin/fogbow-cli token --get --password mypassword --username myusername --tenantName mytenantname --url http://localhost:8182

MIINXgYJKoZIhvcNAQcCoIINTzCCDUsCAQExCTAHBgUrDgMCGjCCC7QGCSqGSIb3DQEHAaCCC6UEgguheyJhY2Nlc3MiOiB7InRva2VuIjogeyJpc3N1ZWRfYXQiOiAiMjAxNC0wNS0
```

### 4 – Request Operations 
Get, create and delete requests.

#### 4.1 – Get request 

##### 4.1.1 – Get all requests

Get all requests user.

* request (Required)
* --url (Required) : Endpoint
* --get (Required)
* --auth-token (Required) : User's token

Example :
```bash
$ bin/fogbow-cli request --get --auth-token mytoken --url http://localhost:8182

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
X-OCCI-Location: http://localhost:8182/request/fd745806-4909-4a39-8380-13183b1f197c
```
##### 4.1.2 – Get request information

Get complete request information.

* request (Required)
* --url (Required) : Endpoint
* --get (Required)
* --auth-token (Required) : User's token
* --id (Required) : Request id

Example :
```bash
$ bin/fogbow-cli request --get --auth-token mytoken --id requestid --url http://localhost:8182

RequestId=47536d31-0674-4278-ad05-eff5fdd07257; State=open; InstanceId=232135435-5435345-435345435-43545
```
#### 4.2 - Create requests 

Create requests.

* request (Required)
* --url (Required) : Endpoint
* --create (Required)
* --auth-token (Required) : User's token
* --n (Optional) : Number of instances
* --image (Optional) : Image Fogbow
* --flavor (Optional) : Flavor Fogbow

Example :
```bash
$ bin/fogbow-cli request --create --n 2 --image myimage --flavor  myflavor --url http://localhost:8182

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
X-OCCI-Location: http://localhost:8182/request/fd745806-4909-4a39-8380-13183b1f197c
```
##### 4.2.1 Default values 

If the user does not fill the optional fields, the default values will be used.

- n = 1
- image = fogbow-linux-x86
- flavor = fogbow-small

Example :
```bash
$ bin/fogbow-cli request --create --url http://localhost:8182

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
```
#### 4.3 - Delete requests 

##### 4.3.1 – Delete all requests

Delete all user's requests.

* request (Required)
* --url (Required) : Endpoint
* --delete (Required)
* --auth-token (Required) : Token user

Example :
```bash
$ bin/fogbow-cli request --delete --auth-token mytoken --url http://localhost:8182

Ok
```

##### 4.3.2 - Delete a request

Delete a user's request.

* request (Required)
* --url (Required) : Endpoint
* --delete (Required)
* --auth-token (Required) : User's token
* --id (Required) : Request id

Example :
```bash
$ bin/fogbow-cli request --delete --auth-token mytoken --id requestid --url http://localhost:8182

Ok
```

### 5 - Instance Operations

Manage instances.

####5.1 - Get instance

#####5.1.1 -  Get all instances

Get all user's instances.

* instance (Required)
* --url (Required) : Endpoint
* --get  (Required)
* --auth-token  (Required) : User's token

Example :
```bash
$ bin/fogbow-cli instance --get --auth-token  mytoken --url http://localhost:8182

X-OCCI-Location: 3I235356-3432434-324324-3243242f
X-OCCI-Location: 4B869582-8907667-123457-0765345c
```
#####5.1.2 -  Get instance information

Get information about an instance.

* instance (Required)
* --url (Required) : Endpoint
* --get  (Required)
* --auth-token  (Required) : User's token
* --id (Required) : Instance id

Example : 
```bash
$ bin/fogbow-cli instance --get --auth-token mytoken --id instanceid --url http://localhost:10000

Category: compute; scheme="http://compute"; class="kind"; title="title"; rel="rel"; location="location"
Category: compute; scheme="http://compute"; class="kind"; title="title"; rel="rel"; location="location"
Link: </network/admin>; org.openstack.compute.console.vnc="N/A"; occi.compute.architecture="x86"; occi.compute.speed="0.0"
X-OCCI-Attribute: org.openstack.compute.console.vnc="N/A"
X-OCCI-Attribute: occi.compute.architecture="x86"
X-OCCI-Attribute: occi.compute.speed="0.0"
```

####5.2 - Delete instances

#####5.2.1 Delete all instances

Delete all user's instances.

* instance (Required)
* --url (Required) : Endpoint
* --delete  (Required)
* --auth-token  (Required) : User's token

```bash
$ bin/fogbow-cli instance --delete --auth-token mytoken --url http://localhost:8182

Ok
```
#####5.2.2 Delete an instance

Delete a user's instance.

* instance (Required)
* --url (Required) : Endpoint
* --delete  (Required)
* --auth-token  (Required) : User's token
* --id (Required) : Instance id

```bash
$ bin/fogbow-cli instance --delete --auth-token mytoken --id instanceid --url http://localhost:8182

Ok
```
