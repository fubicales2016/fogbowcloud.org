Title: Fogbow CLI
url: fogbow-cli
save_as: fogbow-cli.html
section: usage
index: 2

Fogbow CLI
==========

The fogbow CLI is a command line interface for the fogbow manager. It makes it easier for fogbow users to create HTTP requests and invoke them through the manager's OCCI API. Through the fogbow CLI, users are able to get information about federation members; create, retrive and delete instance, network, create storage and create orders.

##Installation
Follow these steps, <a  href="/install-configure-manager" target="_blank">Instalation and configuration Fogbow cli</a>

## Member operations (```member```)

### List federation members 

Get all federation members.

* **--get** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint

Example:
```bash
$ fogbow-cli member --get --url http://localhost:8182

federation.member.one.com
federation.member.two.com
federation.member.three.com
```

## Resource operations (```resource```)

### Get all fogbow resources 

Get all OCCI resources provided by fogbow. 

* **--get** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (required): user's token

### Get quota
Get the quota of the federation member

* **--get** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--quota** (required)
* **--memberId** (required)

Example:
```bash
$ fogbow-cli member --get --url http://localhost:8182 --quota --memberId id123

cpuQuota=1;cpuInUse=1;cpuInUseByUser=1;memQuota=1;memInUse=1;memInUseByUser=1;instancesQuota=1;instancesInUse=1;instancesInUseByUser=1
```

### Get usage
Get the usage of the federation member

* **--get** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--usage** (required)
* **--memberId** (required)

Example:
```bash
$ fogbow-cli member --get --url http://localhost:8182 --usage --memberId id123

memberId=federation.member.one.com
compute usage=10
```

Example:
```bash
$ fogbow-cli resource --get --url http://localhost:8182 --auth-token mytoken

Category: fogbow-request; scheme="http://schemas.fogbowcloud.org/request#"; class="kind"; title="Request new Instances"; location="http://localhost:8182/request"; attributes="org.fogbowcloud.request.instance-count org.fogbowcloud.request.type org.fogbowcloud.request.valid-until org.fogbowcloud.request.valid-from"
Category: fogbow-large; scheme="http://scmhemas.fogbowcloud.org/template/resource#"; class="mixin"; title="Large Flavor"; location="http://localhost:8182/large"
Category: fogbow-linux-x86; scheme="http://schemas.fogbowcloud.org/template/os#"; class="mixin"; title="Linux-x86 Image"; location="http://localhost:8182/fogbow-linux-x86"
```

## Token operations (```token```)

### Create a new Token

Create a new user token.

Note: to pass the credentials and the identity plugin endpoint it is necessary the use of dynamic parameters; follow the example with the OpenStack credentials:

* **--create** (required)
* **--type** (required) : identity plugin type
* **-DauthUrl** (required): identity plugin endpoint
* **-Dpassword=** (optional): dynamic parameter
* **-Dusername=** (optional): dynamic parameter
* **-DtenantName=** (optional): dynamic parameter

Note: if a password is not provided, it will be requested in the console.

Example:
```bash
$ fogbow-cli token --create -Dpassword=mypassword -Dusername=myusername -DtenantName=mytenantname -DauthUrl=http://localhost:8182 --type openstack

MIINXgYJKoZIhvcNAQcCoIINTzCCDUsCAQExCTAHBgUrDgMCGjCCC7QGCSqGSIb3DQEHAaCCC6UEgguheyJhY2Nlc3MiOiB7InRva2VuIjogeyJpc3N1ZWRfYXQiOiAiMjAxNC0wNS0
```

## Request operations (```request```)

### Get request 

Get all instance requests associated to a particular user's token.

* **--get** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (required): user's token

Example:
```bash
$ fogbow-cli request --get --auth-token mytoken --url http://localhost:8182

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
X-OCCI-Location: http://localhost:8182/request/fd745806-4909-4a39-8380-13183b1f197c
```

Get detailed information about a single instance request.

* **--get** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (required): user's token
* **--id** (required): request id

Example:
```bash
$ fogbow-cli request --get --auth-token mytoken --id requestid --url http://localhost:8182

RequestId=47536d31-0674-4278-ad05-eff5fdd07257; State=open; InstanceId=232135435-5435345-435345435-43545
```

### Create requests 

Create instance requests.

* **--create** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (required): user's token
* **--n** (optional; default: 1): number of requests to be created
* **--image** (optional; default: fogbow-linux-x86): fogbow image
* **--flavor** (optional; default: fogbow-small): fogbow flavor
* **--public-key** (optional; path public key): fogbow public key

Example:
```bash
$ fogbow-cli request --create --n 2 --image fogbow-linux-x86 --flavor large --url http://localhost:8182 --public-key ~/.ssh/id_rsa.pub

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
X-OCCI-Location: http://localhost:8182/request/fd745806-4909-4a39-8380-13183b1f197c
```

Example using defaults:
```bash
$ fogbow-cli request --create --url http://localhost:8182

X-OCCI-Location: http://localhost:8182/request/47536d31-0674-4278-ad05-eff5fdd07257
```

### Delete all requests

Delete all instance requests associated to a particular user's token.

* **--delete** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (required): user's token

Example:
```bash
$ fogbow-cli request --delete --auth-token mytoken --url http://localhost:8182

Ok
```

### Delete a single request

Delete a single instance request.

* **--delete** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (required): user's token
* **--id** (required): request id

Example:
```bash
$ fogbow-cli request --delete --auth-token mytoken --id requestid --url http://localhost:8182

Ok
```

## Instance operations (```instance```)

### Get all instances

Get all instances associated to a particular user's token.

* **--get**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (required): user's token

Example:
```bash
$ fogbow-cli instance --get --auth-token  mytoken --url http://localhost:8182

X-OCCI-Location: 3I235356-3432434-324324-3243242f
X-OCCI-Location: 4B869582-8907667-123457-0765345c
```

### Get a single instance

Get detailed information about a single instance.

* **--get**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (required): user's token
* **--id** (required): instance id

Example: 
```bash
$ fogbow-cli instance --get --auth-token mytoken --id instanceid --url http://localhost:10000

Category: compute; scheme="http://compute"; class="kind"; title="title"; rel="rel"; location="location"
Category: compute; scheme="http://compute"; class="kind"; title="title"; rel="rel"; location="location"
Link: </network/admin>; org.openstack.compute.console.vnc="N/A"; occi.compute.architecture="x86"; occi.compute.speed="0.0"
X-OCCI-Attribute: org.openstack.compute.console.vnc="N/A"
X-OCCI-Attribute: occi.compute.architecture="x86"
X-OCCI-Attribute: occi.compute.speed="0.0"
X-OCCI-Attribute: org.fogbowcloud.request.ssh-address="10.1.0.43:5000"
```

### Delete all instances

Delete all instances associated to a particular user's token.

* **--delete**  (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (required): user's token

```bash
$ fogbow-cli instance --delete --auth-token mytoken --url http://localhost:8182

Ok
```

### Delete a single instance

Delete a single instance.

* **--delete** (required)
* **--url** (optional; default: http://localhost:8182): OCCI endpoint
* **--auth-token** (required): user's token
* **--id** (required): instance id

```bash
$ fogbow-cli instance --delete --auth-token mytoken --id instanceid --url http://localhost:8182

Ok
```
