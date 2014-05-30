Title: Manager client
url: manager-cli
save_as: manager-cli.html
section: usage
index: 0

Manager CLI
==========

The Manager CLI is a command line interface with the fogbow manager. It makes it easier for fogbow users to create HTTP requests to the manager OCCI API. Through the Manager CLI, users are able to get information about federation members, to create instance requests and to manage the lifecycle of those requests.

## Member operations (```member```)

### List Federation members 

Get all federation members.

* **--get** (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint

Example :
```bash
$ bin/fogbow-cli member --get --url http://localhost:8182

id=ferationid1;cpuIdle=1;cpuInUse=2;memIdle=100;memInUse=200;flavor:'small, capacity="1"';
id=ferationid2;cpuIdle=2;cpuInUse=4;memIdle=150;memInUse=300;flavor:'large, capacity="2"';
```


## Resource operations (```resource```)

### Get all fogbow Resources 

Get all OCCI resources provided by fogbow. 

* **--get** (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint

Example :
```bash
$ bin/fogbow-cli resource --get --url http://localhost:8182

Category: fogbow-request; scheme="http://schemas.fogbowcloud.org/request#"; class="kind"; title="Request new Instances"; location="http://localhost:8182/request"; attributes="org.fogbowcloud.request.instance-count org.fogbowcloud.request.type org.fogbowcloud.request.valid-until org.fogbowcloud.request.valid-from"
Category: fogbow-large; scheme="http://schemas.fogbowcloud.org/template/resource#"; class="mixin"; title="Large Flavor"; location="http://localhost:8182/large"
Category: fogbow-linux-x86; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin"; title="Linux-x86 Image"; location="http://localhost:8182/fogbow-linux-x86"
```


## Token operations (```token```)

### Create a new Token

Create a new user token.

* **--get** (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint
* **--password** (Optional) 
* **--username** (Required)
* **--tenantName** (Required) 

Obs.: If a password is not provided, it will be requested in the console.

Example :
```bash
$ bin/fogbow-cli token --get --password mypassword --username myusername --tenantName mytenantname --url http://localhost:8182

MIINXgYJKoZIhvcNAQcCoIINTzCCDUsCAQExCTAHBgUrDgMCGjCCC7QGCSqGSIb3DQEHAaCCC6UEgguheyJhY2Nlc3MiOiB7InRva2VuIjogeyJpc3N1ZWRfYXQiOiAiMjAxNC0wNS0
```


## Request Operations (```request```)

### Get request 

Get all instance requests of an user.

* **--get** (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint
* **--auth-token** (Required) : User's token

Example :
```bash
$ bin/fogbow-cli request --get --auth-token mytoken --url http://localhost:8182

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
X-OCCI-Location: http://localhost:8182/request/fd745806-4909-4a39-8380-13183b1f197c
```

Get detailed information about a single instance request.

* **--get** (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint
* **--auth-token** (Required) : User's token
* **--id** (Required) : Request id

Example :
```bash
$ bin/fogbow-cli request --get --auth-token mytoken --id requestid --url http://localhost:8182

RequestId=47536d31-0674-4278-ad05-eff5fdd07257; State=open; InstanceId=232135435-5435345-435345435-43545
```


### Create requests 

Create instance requests.

* **--create** (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint
* **--auth-token** (Required) : User's token
* **--n** (Optional; Default: 1) : Number of requests to be created
* **--image** (Optional; Default: fogbow-linux-x86) : Fogbow image
* **--flavor** (Optional; Default: fogbow-small) : Fogbow flavor

Example :
```bash
$ bin/fogbow-cli request --create --n 2 --image fogbow-linux-x86 --flavor large --url http://localhost:8182

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
X-OCCI-Location: http://localhost:8182/request/fd745806-4909-4a39-8380-13183b1f197c
```

Example using defaults:
```bash
$ bin/fogbow-cli request --create --url http://localhost:8182

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
```


### Delete all requests

Delete all instance requests of an user.

* **--delete** (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint
* **--auth-token** (Required) : User's token

Example :
```bash
$ bin/fogbow-cli request --delete --auth-token mytoken --url http://localhost:8182

Ok
```


### Delete a single request

Delete a single instance request.

* **--delete** (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint
* **--auth-token** (Required) : User's token
* **--id** (Required) : Request id

Example :
```bash
$ bin/fogbow-cli request --delete --auth-token mytoken --id requestid --url http://localhost:8182

Ok
```


## Instance operations (```instance```)

### Get all instances

Get all instances created by an user.

* **--get**  (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint
* **--auth-token** (Required) : User's token

Example :
```bash
$ bin/fogbow-cli instance --get --auth-token  mytoken --url http://localhost:8182

X-OCCI-Location: 3I235356-3432434-324324-3243242f
X-OCCI-Location: 4B869582-8907667-123457-0765345c
```


### Get a single instance

Get detailed information about a single instance.

* **--get**  (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint
* **--auth-token** (Required) : User's token
* **--id (Required)** : Instance id

Example : 
```bash
$ bin/fogbow-cli instance --get --auth-token mytoken --id instanceid --url http://localhost:10000

Category: compute; scheme="http://compute"; class="kind"; title="title"; rel="rel"; location="location"
Category: compute; scheme="http://compute"; class="kind"; title="title"; rel="rel"; location="location"
Link: </network/admin>; org.openstack.compute.console.vnc="N/A"; occi.compute.architecture="x86"; occi.compute.speed="0.0"
X-OCCI-Attribute: org.openstack.compute.console.vnc="N/A"
X-OCCI-Attribute: occi.compute.architecture="x86"
X-OCCI-Attribute: occi.compute.speed="0.0"
X-OCCI-Attribute: org.fogbowcloud.request.ssh-address="10.1.0.43:5000"
```


### Delete all instances

Delete all instances created by an user.

* **--delete**  (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint
* **--auth-token** (Required) : User's token

```bash
$ bin/fogbow-cli instance --delete --auth-token mytoken --url http://localhost:8182

Ok
```


### Delete a single instance

Delete a single instance.

* **--delete**  (Required)
* **--url** (Optional; Default: http://localhost:8182): OCCI endpoint
* **--auth-token** (Required) : User's token
* **--id** (Required) : Instance id

```bash
$ bin/fogbow-cli instance --delete --auth-token mytoken --id instanceid --url http://localhost:8182

Ok
```
