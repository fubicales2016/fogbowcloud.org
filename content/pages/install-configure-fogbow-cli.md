Title: Install and configure Fogbow 
url: intall-configure-fogbow-cli
save_as: intall-configure-fogbow-cli.html
section: install-configure
index: 2

Install and configure Fogbow CLI
==========

The fogbow CLI is a command line interface for the fogbow manager. It makes it easier for fogbow users to create HTTP requests. Through the fogbow CLI, users are able to get information about federation members, order resources and manage the lifecycle of those resources.

##Installation
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
