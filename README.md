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
    docker run --volumes-from $OVPN_DATA -d -p 1194:1194/udp --cap-add=NET_ADMIN kylemanna/openvpn
    docker run --volumes-from $OVPN_DATA --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass
    docker run --volumes-from $OVPN_DATA --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn

Once you're set up, move openvpn.service to /etc/systemd/system/ and install the new service with:

    $ sudo systemctl enable /etc/systemd/system/openvpn.service
    $ sudo systemctl start openvpn.service

For more info, see [Getting Started with systemd](https://coreos.com/docs/launching-containers/launching/getting-started-with-systemd/).
