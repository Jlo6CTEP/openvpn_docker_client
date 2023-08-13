This project allows setting up openvpn client with docker. There is also a version that is able to hide your VPN traffic behind TLS for extra security and stability against opressive governments' attempts to disrupt VPN connections.

## Setup
* Put your ovpn cert into `./openvpn/certs`
* Start the container with `docker-compose up -d`
* Enjoy
## VPN over TLS
In this case you will need a little bit more setup in addition to your regular ovpn cert
* Put your `CA.pem` which is your root CA cert into `./stunnel/certs/`
* Don't forget to update `./stunnel/stunnel.conf` file with the ip address of your instance
* Start with `docker compose -f docker-compose.yaml -f docker-compose-tls.yaml up -d`
* Enjoy

Beware: your openvpn provider should support VPN over TLS on their end and provide you with both ovpn cert and CA cert.
You can check out my [repo](https://github.com/Jlo6CTEP/openvpn-pihole) if you want to set up one for yourself. It is based on [simonwep](https://github.com/simonwep)'s project with added VPN-over-TLS capability on top.
