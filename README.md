This project allows setting up OpenVPN client with docker. There is also a version that is able to hide your VPN traffic behind TLS for extra security and stability against opressive governments' futile attempts to disrupt VPN connections.

## Setup
* Put your ovpn cert into `./openvpn/certs`
* Start the container with `docker-compose up -d`
* Enjoy
## VPN over TLS
In this case you will need a little bit more setup in addition to your regular ovpn cert
* Put your `ca.pem` which is your root CA cert into `./stunnel/certs/`
* Don't forget to update `./stunnel/stunnel.conf` file with the ip address of your instance (it can be found near the beginning of ovpn cert `route <this_is_your_ip> 255.255.255.255 net_gateway`)
* Start with `docker compose -f docker-compose.yaml -f docker-compose-tls.yaml up -d`
* Enjoy

Beware: your OpenVPN provider should support VPN over TLS on their end and provide you with both ovpn cert and CA cert.
You can check out my [repo](https://github.com/Jlo6CTEP/openvpn-pihole) if you want to set up one for yourself. It is based on [simonwep](https://github.com/simonwep)'s project with added VPN-over-TLS capability on top.
You can obtain `ca.pem` file from the contents of `openvpn-pihole/openvpn/pki/ca.crt`. Just copy this file's content into `ca.pem` and you should be good to go

## Docker-capable systems other than Linux
This will work on any system that supports docker, however there are few caveats to it
* On MacOS, docker spins up a lightweight linux VM, so the `network_mode: host` will work in the context of that VM, not in the host machine. 
 Which means that if you want to access something in the internet, you should do it from the VM. I have no idea whether it is possible or not
* On Windows (with WSL2) the same applies, but here your VM is WSL itself, so, you can easily access your development tools across the private network, and even install browser and surf the Web, if you have WSLg configured which you probably should by default with the newer Ubuntu distros. 
You might also want to enable systemd for your WSL distro, because stunnel4 uses it
  * Howerver, sometimes it does not create tun/tap device on host, I do not know why yet

## Without docker
If you can not use docker, you should install everything on the host machine instead
### Linux
The process is fairly straightforward
* Obtain ovpn and ca.pem files
* Obtain stunnel.conf file from this repo and update ip
* Install OpenVPN and stunnel4 `sudo apt-get install openvpn` and `sudo apt-get install stunnel4`
* Stick your `ca.pem` and `stunnel.conf` to `/etc/stunnel/`
* Start stunnel service `sudo service stunnel4 start`
* Start OpenVPN with `sudo openvpn --config <yor_config.ovpn>`
### Windows
The process is similar to one in linux version
* Grab all the needed files (in this case you will need `stunnel.conf` from `win` folder)
  * Don't forget to change the ip! 
  * You should also comment out (put # in the beginning of the line) lines with `up /etc/openvpn/update-resolv-conf` and `down /etc/openvpn/update-resolv-conf`
* Install OpenVPN / OpenVPN connect through `winget install --id OpenVPNTechnologies.OpenVPN`
  * If you don't have `winget` or do not how how to use it, you can also download OpenVPN from their official site.
* Install [stunnel](https://www.stunnel.org/downloads.html), it is not on winget so you have to resort to inferior Windows-way of installing software. i.e. through GUI
  * Accept all the defaults in GUI and in the command line if one pops up (just hit Enter a bunch of times) 
* Put your `ca.pem` and `stunnel.conf` to `C:/Program Files (x86)/stunnel/config/`
* Since managing anything is a major clusterfuck if we are talking Windows, go ahead and launch a *separate* program called `stunnel GUI start` to make sure that everything is ok and stunnel in fact starts and works as intended
  * You can find this and other `stunnel` programs in the Start menu (Win key) 
* Now we will make sure that it starts when the system starts and keeps running no matter what
* You have a *separate* program called `stunnel GUI stop` to stop your stunnel, you want to do this to free the port
* Install a service through `stunnel service install` which is, you guessed it, yet another *separate* program
* Start a service with `stunnel service start`
* Now you should be able to connect to VPN with your OpenVPN client
* Optional: set 172.20.0.2 as your default DNS server (this will also cut annoying ads btw) and will allow opening sites that are DNS-level blocked

### Android
TBD  

### MacOS
Probably not TBD, because I have no MacOS devices. Should work similarly to Windows / Linux though 
