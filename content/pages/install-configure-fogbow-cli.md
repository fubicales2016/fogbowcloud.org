Title: Install and configure Fogbow 
url: intall-configure-fogbow-cli
save_as: intall-configure-fogbow-cli.html
section: install-configure
index: 2

Install and configure Fogbow CLI
==========

The Fogbow Manager is controlled via a command line interface (CLI) that makes easier for the fogbow users to get information about the federation members, to order resources and to manage the lifecycle of those resources.

##Installation
The Fogbow CLI can be installed from its source or from from a binary package for debian-based distributions. Download the best choice for your systems and install the CLI as follow.

###Install from source
Get the latest code and install it with maven.
``` bash
git clone https://github.com/fogbow/fogbow-cli.git
cd fogbow-cli
mvn install
```

###Install from debian package
Download the [lastest stable package](http://downloads.fogbowcloud.org/nightly/debian/fogbow-cli/fogbow-cli_latest.deb)
```bash
wget http://downloads.fogbowcloud.org/nightly/debian/fogbow-cli/fogbow-cli_latest.deb
```
And install it with dpkg
```bash
sudo dpkg -i fogbow-cli_latest.deb
```
