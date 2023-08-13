This project allows setting up openvpn client with docker. There is also a version that is able to hide your VPN traffic behind TLS for extra security and stability against opressive governments' attempts to disrupt VPN connections.

## Setup
* Put your ovpn cert into `./openvpn/certs`
* Start the container with `docker-compose up -d`
* Enjoy
## VPN over TLS
In this case you will need a little bit more setup in addition to your regular ovpn cert
* Put your `ca.pem` which is your root CA cert into `./stunnel/certs/`
* Don't forget to update `./stunnel/stunnel.conf` file with the ip address of your instance
* Start with `docker compose -f docker-compose.yaml -f docker-compose-tls.yaml up -d`
* Enjoy

Beware: your openvpn provider should support VPN over TLS on their end and provide you with both ovpn cert and CA cert.
You can check out my [repo](https://github.com/Jlo6CTEP/openvpn-pihole) if you want to set up one for yourself. It is based on [simonwep](https://github.com/simonwep)'s project with added VPN-over-TLS capability on top.
You can obtain `ca.pem` file from the contents of `openvpn-pihole/openvpn/pki/ca.crt`. Just copy this file content into `ca.crt` and you should be good to go

## Docker-capable systems other than Linux
This will work on any system that supports docker, however there are few caveats to it
* On MacOS, docker spins up a lightweight linux VM, so the `network_mode: host` will work in the context of that VM, not in the host machine
Which means that if you want to access something in the internet, you should do it from the VM. I have no idea whether it is possible or not
* On Windows (with WSL2) the same applies, but here your VM is WSL itself, so, you can easily access your development tools across the private network, and even install browser and surf the Web, if you have WSLg configured which you probably should by default with the newer Ubuntu distros. 
You might also want to enable systemd for your WSL distro

## Without docker
If you can not use this from docker, you should install everything on the host machine instead
### Linux
The process is fairly straightforward
* Obtain ovpn and ca.pem files
* Obtain stunnel.conf file from this repo and update ip
* Install openvpn and stunnel4 `sudo apt-get install openvpn` and `sudo apt-get install stunnel4`
* Stick your `ca.pem` and `stunnel.conf` to `/etc/stunnel/`
* Start stunnel service `sudo service stunnel4 start`
* Start openvpn with `sudo openvpn --config <yor_config.ovpn>`
### Windows
TBD
### MacOS
Probably not TBD, because I have no MacOS devices. Should work similarly to Windows / Linux one though 
