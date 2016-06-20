Title: Install and configure fogbow reverse tunnel
url: install-configure-reverse-tunnel
save_as: install-configure-reverse-tunnel.html
section: install-configure
index: 6

Install and configure reverse tunnel
==========

A reverse tunneling service provides public IP access to all virtual machines created in the private clouds, even if the local cloud offers only private IPs to these virtual machines.

TODO: precisamos explicar que na verdade, temos multiplos servidores. na configuracao precisamos deixar claro que é especificado o numero de 

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
Download to the <a href="http://downloads.fogbowcloud.org/nightly/debian/fogbow-reverse-tunnel/fogbow-reverse-tunnel_latest.deb" target=_blank>latest debian package</a>.
```bash
wget http://downloads.fogbowcloud.org/nightly/debian/fogbow-reverse-tunnel/fogbow-reverse-tunnel_latest.deb
```

Then, install it with dkpg
```bash
dpkg -i fogbow-reverse-tunnel_latest.deb
```

TODO: acho que precisamos especificar os diretorios em que o package será instalado (na verdade, onde estarão os arquivos de config)

## Configure
After the installation, rename the file ```reverse-tunnel.conf.example``` to ```reverse-tunnel.conf``` and edit its contents:
```bash
# The range of ports used for creation of tunnel's server .
# These ports defines the number of servers to be used
tunnel_port_range=2224:2242

# Number of ports to be allocated in each server.
ports_per_ssh_server=5

# This property defines the hosts address of Reverse Tunnel's server
# 0.0.0.0 sets the server to listen on all interfaces
tunnel_host=0.0.0.0

# Http port of the Fogbow Reverse Tunnel.
# Port to run the HTTP service of the Reverse Tunnel
# Where the manager should do requests to create or get ports to use in tunnel
http_port=2223

# The range of external ports.
# valore nao casam
external_port_range=20000:30000

# Path to save the generated key pair for the SSH Server
# It should be defined to the server use the same key even after restart
host_key_path=hostkey.ser

# Checking time of token.
# Amount of time before mark a token as expired
idle_token_timeout=86400

# Checking of the interval of idle ports in the tunnel 's server.
# Interval between the check for idle tokens in each server to remove the tunnel
check_ssh_servers_interval=120
```

## Run
To start the reverse tunnel server, run the start-tunnel-server script inside ```./bin```.
``` shell
./bin/start-tunnel-server
```
