# unit-openvpn
Openvpn unit for coreos based on kylemanna/docker-openvpn

#Installation
To set up the openvpn server, start here: [https://github.com/kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn)

Once you're set up, move openvpn.service to /etc/systemd/system/ and install the new service with:

    $ sudo systemctl enable /etc/systemd/system/hello.service
    $ sudo systemctl start hello.service

For more info, see [https://coreos.com/docs/launching-containers/launching/getting-started-with-systemd/](https://coreos.com/docs/launching-containers/launching/getting-started-with-systemd/).
