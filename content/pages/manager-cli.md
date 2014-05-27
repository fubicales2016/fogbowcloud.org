Title: Manager cli
url: manager-cli
save_as: manager-cli.html
section: usage
index: 0

Manager command line interface
==========

Manager command line interface is the service for the client that should be use to execute functions the system. It allows user know about federation members, resources provided by fogbow and get, create and delete requests and get and delete instances. 

### Summary
<ol>
  <li>Member Operations</li>
  <li>Resources Operations</li>
  <li>Token Operations</li>
  <li>Request Operations</li>
  <li>Instance Operations</li>
</ol>

=========
### 1 - Member Operations. 
<ul>
  <li>member (Required)</li>
  <li>--url (Required) : Endpoint</li>
</ul>

#### 1.1 - List Federation members 
Get all federation members.
<ul>
  <li>--get (Required)</li>
</ul>
``` shell
Example : member --get --url http://url.com:10000
```

=========
### 2 - Resources Operations
<ul>
  <li>resource (Required)</li>
  <li>--url (Required) : Endpoint</li>
</ul>
#### 2.1 - Get all Fogbow Resources 
Get all resources provided by Fogbow.
<ul>
  <li>--get (Required)</li>
</ul>
``` shell
Example : resource --get --url http://url.com:10000
```

=========
### 3 – Get new Token
<ul>
  <li>token (Required) : Token user.</li>
  <li>--url (Required) : Endpoint</li>
</ul>

#### 3.1 - Get a new Token
Get a new Token.
<ul>
  <li>--get (Required)</li>
  <li>--password (Optional)</li>
  <li>--username (Required)</li>
  <li>--tenantName (Required)</li>
</ul>
``` shell
Example : token --get --password mypassword --username myusername --tenantName mytenantname --url http://url.com:10000
```
=========
### 4 – Request Operations 
Get, create and delete requests.
<ul>
  <li>request (Required)</li>
  <li>--url (Required) : Endpoint</li>
</ul>

#### 4.1 – Get request 
##### 4.1.1 – Get all requests
Get all requests from user.
<ul>
  <li>--get (Required)</li>
  <li>--auth-token (Required) : Token user.</li>
</ul>
``` shell
Example : request --get --auth-token mytoken --url http://url.com:10000
```
##### 4.1.2 – Get specific request
Get specific request from user.
<ul>
  <li>--get (Required)</li>
  <li>--auth-token (Required) : Token user.</li>
  <li>--id (Required) : Request id.</li>
</ul>
``` shell
Example : request --get --auth-token mytoken --id requestid --url http://url.com:10000
```
####4.2 - Create requests 
Create requests.
<ul>
  <li>--create (Required) </li>
  <li>--auth-token (Required) : Token user.</li>
  <li>--n (Optional) : Number of request.</li>
  <li>--image (Optional)</li>
  <li>--flavor (Optional)</li>
</ul>
``` shell
Example : request --create --n 2 --image myimage --flavor  myflavor --url http://url.com:10000
```
##### 4.2.1 Values Default 
If the User does not fill the optional fields, the default values will be used.
<ul>
  <li>--n = 1 </li>
  <li>--image = fogbow-linux-x86</li>
  <li>--flavor = fogbow-small </li>
</ul>
``` shell
Example : request --create --url http://url.com:10000  
```
#### 4.3 - Delete requests 
##### 4.3.1 – Delete all requests
Delete all requests user.
<ul>
  <li>--delete (Required)</li>
  <li>--auth-token (Required) : Token user.</li>
</ul>
``` shell
Example : request --delete --auth-token mytoken --url http://url.com:10000
```
##### 4.3.2 - Delete specific requests
Delete specific request user.
<ul>
  <li>--delete (Required)</li>
  <li>--auth-token (Required) : Token user.</li>
  <li>--id (Required) : Request id.</li>
</ul>
``` shell
Example : request --delete --auth-token mytoken --id requestid --url http://url.com:10000
```
=========
### 5 - Instance Operations
<ul>
  <li>instance (Required)</li>
  <li>--url (Required) : Endpoint</li>
</ul>
####5.1 - Get instance
#####5.1.1 -  Get all instance
Get all instances user.
<ul>
  <li>--get  (Required)</li>
  <li>--auth-token  (Required) : Token user.</li>
</ul>
``` shell
Example : instance --get --auth-token  mytoken --url http://url.com:10000
```
#####5.1.1 -  Get specific instance
Get specific instance user.
<ul>
  <li>--get  (Required)</li>
  <li>--auth-token  (Required) : Token user. </li>
  <li>--id (Required) : Instance id.</li>
</ul>
``` shell
Example : instance --get --auth-token mytoken --id instanceid --url http://url.com:10000
```
