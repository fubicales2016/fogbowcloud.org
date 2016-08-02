Title: Install and configure Fogbow CLI
url: install-configure-fogbow-cli
save_as: install-configure-fogbow-cli.html
section: install
index: 5

Install and configure the Fogbow CLI
==========

The Fogbow Manager is controlled via a command line interface (CLI) that makes easier for the fogbow users to get information about the federation members, to order resources and to manage the lifecycle of those resources. The Fogbow CLI is distributed in two forms: as source code or as a binary package for debian-based distributions. Choose the best distribution for your system, download it and install it as follow.

##Install from source

To get the lastest stable version of the component, download it from our repository
```shell
wget https://github.com/fogbow/fogbow-cli/archive/master.zip
``` 

Then, decompress it:
```shell
unzip master.zip
```

Now, install it with Maven:
```
cd fogbow-cli-master
mvn install
```

## Install from debian package

Download a stable version from our <a href="http://downloads.fogbowcloud.org/stable/debian/">package repository</a> and install it with dpkg:

```bash
dpkg -i fogbow-cli_$version.deb
```
