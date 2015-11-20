# unit-openvpn
OpenVPN unit for coreos based on kylemanna/docker-openvpn

#Installation
To set up the openvpn server, start here:
[docker-openvpn](https://github.com/kylemanna/docker-openvpn). I use a TCP
connection, so I specify the protocol slightly differently. Below are my setup
steps:

    export OVPN_DATA="ovpn-data"
    docker run --name $OVPN_DATA -v /etc/openvpn busybox
    docker run --volumes-from $OVPN_DATA --rm kylemanna/openvpn ovpn_genconfig -u tcp://VPN.SERVERNAME.COM:443
    docker run --volumes-from $OVPN_DATA --rm -it kylemanna/openvpn ovpn_initpki
    docker run --volumes-from $OVPN_DATA -d -p 443:1194/tcp --cap-add=NET_ADMIN kylemanna/openvpn
    docker run --volumes-from $OVPN_DATA --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass
    docker run --volumes-from $OVPN_DATA --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn

Once you're set up, download openvpn.service to /etc/systemd/system/ and install the new service with:

    $ wget -O /etc/systemd/system/openvpn.service https://raw.githubusercontent.com/mypetyak/unit-openvpn/master/openvpn.service
    $ sudo systemctl enable /etc/systemd/system/openvpn.service
    $ sudo systemctl start openvpn.service

Then, on your local machine, use `scp` to retrieve CLIENTNAME.ovpn before
deleting this file on the server.

To confirm the service is up and running, check `docker ps`, or see the output
from `journalctl -f -u openvpn.service`.

For more info, see [Getting Started with systemd](https://coreos.com/docs/launching-containers/launching/getting-started-with-systemd/).
