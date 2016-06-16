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
dpkg -i fogbow-reverse-tunnel_latest.deb
```

## Configure
After the installation, rename the file ```reverse-tunnel.conf.example``` to ```reverse-tunnel.conf``` and edit its contents:
```bash
# The range of port used for creation of tunnel's server .
tunnel_port_range=2224:2242

# Amount of port released for each tunnel server.
ports_per_ssh_server=5
tunnel_host=0.0.0.0

# Http port of the Fogbow Reverse Tunnel.
http_port=2223

# The range of external port.
external_port_range=20000:30000

host_key_path=hostkey.ser

# Checking time of token.
idle_token_timeout=86400

# Checking of the interval of idle ports in the tunnel 's server.
check_ssh_servers_interval=120
```

## Run
To start the rendezvous component, run the start-rendezvous script inside ```./bin```.
``` shell
./bin/start-tunnel-server
```
