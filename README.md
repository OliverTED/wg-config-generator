<br />
<div align="center">
  <a href="https://github.com/OliverTED/wg-config-generator">
    <h3 align="center">Writeguard config generator</h3>
  </a>


  <p align="center">
    Simple script to generate wireguard scripts for a simple star topology configuration.
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template">View Demo</a>
    ·
    <a href="https://github.com/othneildrew/Best-README-Template/issues">Report Bug</a>
    ·
    <a href="https://github.com/othneildrew/Best-README-Template/issues">Request Feature</a>
  </p>
</div>


# Writeguard Config Generator

## About

<a href="https://github.com/OliverTED/wg-config-generator">Writeguard
Config Generator</a> is a simple solution to generate a wireguard
configuration files.  Configuration is only generated, uploading to
the hosts is not automated.

Currently supported is a simple configuration where there is a single
public forwarding server (e.g. an ec2 instance) and various clients
which connect to it.  (add your own use cases!)

## Quick start

Create a simple configuration file and store as 'config.yaml':

```yaml
server:
  name: my_net  # interface name
  ip: 10.10.0.1  # server's ip address
  subset: 24  # subnet size, e.g. 11.11.2.0 - 11.11.2.255
  ip_external: 33.200.201.21  # public server address
  port: 51830  # public server port

hosts:
  - {name: client01, ip: 10.10.0.2}  # client 01 name and ip address
  - {name: client02, ip: 10.10.0.3}  # client 02 name and ip address
```

then run wg-config-generator:

```sh
pip install wg-config-generator
wg-config-generator
```

It will automatically create wireguard private keys and store them in
'config.privkeys.yaml'.  The configuration is stored in 'build/' where
there is a single wireguard configuration for each host, such as.

```yaml
# save this file as
#> sudo cp my_net.client01.10.10.0.2.conf /etc/wireguard/my_net.conf; sudo chmod 600 /etc/wireguard/my_net.conf
#
# test this connection with
#> sudo wg-quick up my_net; sudo wg; sudo wg-quick down my_net
#
# manage autostarts with
#> sudo systemctl enable wg-quick@my_net
#> sudo systemctl start wg-quick@my_net
#> sudo systemctl stop wg-quick@my_net
#> sudo systemctl disable wg-quick@my_net

[Interface]
Address = 10.10.0.2/24
# SaveConfig = true
ListenPort = 51830
PrivateKey = eHyr0QK954QRBd/7FLWk8EzK4BFjQ3NBqJXPtz24j0M=

# ec2-aisc-wg
[Peer]
PublicKey = nWelyFwx0+pFvs98BtR8akd9Pw6qGr+x3vE4aR4xLCA=
AllowedIPs = 10.10.0.1/24
Endpoint = 33.200.201.21:51830
PersistentKeepalive = 25
```

Put the config files on the hosts as described in the comments of each config.

## Why

Wireguard should be easy to setup without the need to installing
complicated software such as a web-interface on the server.

## Contributing

Add you use cases, especially other network configurations.


## License

Distributed under the MIT License. See `LICENSE.txt` for more information.
