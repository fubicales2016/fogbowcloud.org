Title: Reverse Tunnel
url: install-reverse-tunnel
save_as: install-reverse-tunnel.html
section: install
index: 5

# Reverse Tunnel



## Install from source

To set up the reverse tunnel server, first, get the latest code from github.
```bash
git clone https://github.com/fogbow/fogbow-reverse-tunnel.git
```
Then, install it with Maven
```bash
mvn install
```

## Install from debian package
To set up the reverse tunnel server, first, download to the [latest debian package](http://downloads.fogbowcloud.org/stable/debian/v0.2.0/fogbow-reverse-tunnel/fogbow-reverse-tunnel_v0.2.0.deb)
```bash
wget http://downloads.fogbowcloud.org/stable/debian/v0.2.0/fogbow-reverse-tunnel/fogbow-reverse-tunnel_v0.2.0.deb
```

Then, install it with dkpg
```bash
dpkg -i fogbow-reverse-tunnel_v0.2.0.deb 
```

## Configure

After the installation, move the file ```reverse-tunnel.conf.example``` to ```reverse-tunnel.conf``` and edit its contents:

```bash
# Port for SSH tunnels
tunnel_port=2222

# Port for HTTP requests
http_port=2223

# IP that the service will attach to
tunnel_host=0.0.0.0

# Range for external ports
external_port_range=40000:50000

# Path for host key file
host_key_path=hostkey.ser

```

## Run

### If installed from source

Execute the start-tunnel-server script:

```bash
./start-tunnel-server
```

### If installed via the debian package

The Fogbow Reverse Tunnel will start automatically after the installation. To restart it, do the following:

```bash
service fogbow-reverse-tunnel start
```
