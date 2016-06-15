Title: Install and configure fogbow reverse tunnel
url: install-configure-reverse-tunnel
save_as: install-configure-reverse-tunnel.html
section: install-configure
index: 6

Install and configure reverse tunnel
==========

The reverse tunnel component acts ....

## Install from source
Get the latest code of the project.
``` shell
git clone https://github.com/fogbow/fogbow-reverse-tunnel.git
```
Then, install it
``` shell
mvn install
```

## Install from debian package
Download to the <a href="http://downloads.fogbowcloud.org/nightly/debian/fogbow-reverse-tunnel/fogbow-reverse-tunnel_latest.deb" target=_blank>latest debian package</a>
```bash
wget http://downloads.fogbowcloud.org/nightly/debian/fogbow-reverse-tunnel/fogbow-reverse-tunnel_latest.deb
```

Then, install it with dkpg
```bash
dpkg -i ffogbow-reverse-tunnel_latest.deb
```

## Configure
After the installation, rename the file ```reverse-tunnel.conf.example``` to ```reverse-tunnel.conf``` and edit its contents:

## Run
To start the rendezvous component, run the start-rendezvous script inside ```./bin```.
``` shell
./bin/start-tunnel-server
```
