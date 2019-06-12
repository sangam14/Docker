```

node1] (local) root@192.168.0.13 ~
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
43cd6abbf133        bridge              bridge              local
d891c58315c0        host                host                local
17072497635b        none                null                local
[node1] (local) root@192.168.0.13 ~

[node1] (local) root@192.168.0.13 ~
$ docker run -dit --name a1 alpine ash
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
e7c96db7181b: Pull complete
Digest: sha256:769fddc7cc2f0a1c35abb2f91432e8beecf83916c421420e6a6da9f8975464b6
Status: Downloaded newer image for alpine:latest
106f65a3c3a77494ca9f6ba874414f8a93995610f9251bb068fb0b767a2d5b7c
[node1] (local) root@192.168.0.13 ~
$ docker run -dit --name a2 alpine ash
b06208e262126c4290d0fa680d8d7efe057957206e0b3c5e869455ab44601aff

[node1] (local) root@192.168.0.13 ~
$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
b06208e26212        alpine              "ash"               12 seconds ago      Up 10 seconds                           a2
106f65a3c3a7        alpine              "ash"               20 seconds ago      Up 18 seconds                           a1
[node1] (local) root@192.168.0.13 ~

[node1] (local) root@192.168.0.13 ~
$ docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "43cd6abbf13358470b7a28ae1395e310e8ddea380db3261f3771084752d65d2a",
        "Created": "2019-06-12T10:45:26.771253869Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "106f65a3c3a77494ca9f6ba874414f8a93995610f9251bb068fb0b767a2d5b7c": {
                "Name": "a1",
                "EndpointID": "65cf9ac44ba30c675359a0129d1a1c5bd7a2ed1d13289f5fbc198f61c1166887",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            },
            "b06208e262126c4290d0fa680d8d7efe057957206e0b3c5e869455ab44601aff": {
                "Name": "a2",
                "EndpointID": "a7ce04a1d3ba79345e60a14806b507a9a62e43664bd18547216bc04bfbd98c56",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
[node1] (local) root@192.168.0.13 ~
$ docker network inspect none
[
    {
        "Name": "none",
        "Id": "17072497635ba9ed5c94ee576dce33d169bc4a2941e2525a0a9300dd074c8ef7",
        "Created": "2019-06-12T10:45:26.637733781Z",
        "Scope": "local",
        "Driver": "null",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": []
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
[node1] (local) root@192.168.0.13 ~
$ docker network inspect host
[
    {
        "Name": "host",
        "Id": "d891c58315c0cd3a61d694ab8989dbad6ba6a1d33c83c27e7f5200273a3e291e",
        "Created": "2019-06-12T10:45:26.656615308Z",
        "Scope": "local",
        "Driver": "host",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": []
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
[node1] (local) root@192.168.0.13 ~
$ docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "43cd6abbf13358470b7a28ae1395e310e8ddea380db3261f3771084752d65d2a",
        "Created": "2019-06-12T10:45:26.771253869Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "106f65a3c3a77494ca9f6ba874414f8a93995610f9251bb068fb0b767a2d5b7c": {
                "Name": "a1",
                "EndpointID": "65cf9ac44ba30c675359a0129d1a1c5bd7a2ed1d13289f5fbc198f61c1166887",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            },
            "b06208e262126c4290d0fa680d8d7efe057957206e0b3c5e869455ab44601aff": {
                "Name": "a2",
                "EndpointID": "a7ce04a1d3ba79345e60a14806b507a9a62e43664bd18547216bc04bfbd98c56",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
[node1] (local) root@192.168.0.13 ~
$ docker attach a1
/ # ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
3: eth0@if4: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
/ # ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: seq=0 ttl=51 time=1.673 ms
64 bytes from 8.8.8.8: seq=1 ttl=51 time=1.194 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 1.194/1.433/1.673 ms
/ # ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
64 bytes from 172.17.0.3: seq=0 ttl=64 time=0.209 ms
64 bytes from 172.17.0.3: seq=1 ttl=64 time=0.100 ms
64 bytes from 172.17.0.3: seq=2 ttl=64 time=0.110 ms
^C
--- 172.17.0.3 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.100/0.139/0.209 ms
/ # ping a2
ping: bad address 'a2'
/ # read escape sequence
[node1] (local) root@192.168.0.13 ~
$ docker stop a1 a2
a1
a2
[node1] (local) root@192.168.0.13 ~
$ docker rm a1 a2
a1
a2
[node1] (local) root@192.168.0.13 ~
$ docker network create --driver bridge a-net

2a392d1c4b63236e8c12da0b7ddc429d8ca623803141a3906f1f6647cbba4b6c
[node1] (local) root@192.168.0.13 ~
$
[node1] (local) root@192.168.0.13 ~
$ docker network inspect a-net
[
    {
        "Name": "a-net",
        "Id": "2a392d1c4b63236e8c12da0b7ddc429d8ca623803141a3906f1f6647cbba4b6c",
        "Created": "2019-06-12T11:02:56.552096064Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
[node1] (local) root@192.168.0.13 ~
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
2a392d1c4b63        a-net               bridge              local
43cd6abbf133        bridge              bridge              local
d891c58315c0        host                host                local
17072497635b        none                null                local
[node1] (local) root@192.168.0.13 ~
$ docker run -dit --name a1 --network a-net alpine ash
106121cfd6c10687d81c92eef358b12b2f49ab52760ccdc028a4f425ac7cc3db
[node1] (local) root@192.168.0.13 ~
$ docker run -dit --name a2 --network a-net alpine ash
2985a3d6a0e2a09859ac62718dcfda3d56ab722011700b5a588c4b69b2517fe2
[node1] (local) root@192.168.0.13 ~
$ docker run -dit --name a3 --network a-net alpine ash
8e2fb98bc39c1fc73b74c546d7b009cad49aee15c246af27f6094f3951e66521
[node1] (local) root@192.168.0.13 ~
$ docker network connect bridge a3
[node1] (local) root@192.168.0.13 ~
$ docker run -dit --name a4  alpine ash
84a1e47daddad7c05879a868c7345b21263a5c84975648b8a0926328cb1e0ccf

$ docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "43cd6abbf13358470b7a28ae1395e310e8ddea380db3261f3771084752d65d2a",
        "Created": "2019-06-12T10:45:26.771253869Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "84a1e47daddad7c05879a868c7345b21263a5c84975648b8a0926328cb1e0ccf": {
                "Name": "a4",
                "EndpointID": "0dde88790fd5fb54e00a1640ca61d92a24409b4d17514863f0b33a9c68dc3dec",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            },
            "8e2fb98bc39c1fc73b74c546d7b009cad49aee15c246af27f6094f3951e66521": {
                "Name": "a3",
                "EndpointID": "739981ca8082afb15ab05c8f1cf0201bd525edaf84eba0c9b9c83bd74c91497c",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
[node1] (local) root@192.168.0.13 ~
$ docker network inspect a-net
[
    {
        "Name": "a-net",
        "Id": "2a392d1c4b63236e8c12da0b7ddc429d8ca623803141a3906f1f6647cbba4b6c",
        "Created": "2019-06-12T11:02:56.552096064Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "106121cfd6c10687d81c92eef358b12b2f49ab52760ccdc028a4f425ac7cc3db": {
                "Name": "a1",
                "EndpointID": "78574c848c69ce8e84512bb10fc290f32cec5f0814d1e55250ce8e245278fa3d",
                "MacAddress": "02:42:ac:13:00:02",
                "IPv4Address": "172.19.0.2/16",
                "IPv6Address": ""
            },
            "2985a3d6a0e2a09859ac62718dcfda3d56ab722011700b5a588c4b69b2517fe2": {
                "Name": "a2",
                "EndpointID": "226774c357467286d145a3ca3ede542f739b787044528861ae128e9dec4ee361",
                "MacAddress": "02:42:ac:13:00:03",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            },
            "8e2fb98bc39c1fc73b74c546d7b009cad49aee15c246af27f6094f3951e66521": {
                "Name": "a3",
                "EndpointID": "c75df17af22499844ea181c022f2504cec2ce53418a04cf359a0734586d7b793",
                "MacAddress": "02:42:ac:13:00:04",
                "IPv4Address": "172.19.0.4/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]


[node1] (local) root@192.168.0.13 ~
$ docker attach a1
/ # ping a2
PING a2 (172.19.0.3): 56 data bytes
64 bytes from 172.19.0.3: seq=0 ttl=64 time=0.210 ms
64 bytes from 172.19.0.3: seq=1 ttl=64 time=0.095 ms
64 bytes from 172.19.0.3: seq=2 ttl=64 time=0.105 ms
^C
--- a2 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.095/0.136/0.210 ms
/ # ping a3
PING a3 (172.19.0.4): 56 data bytes
64 bytes from 172.19.0.4: seq=0 ttl=64 time=0.164 ms
64 bytes from 172.19.0.4: seq=1 ttl=64 time=0.094 ms
64 bytes from 172.19.0.4: seq=2 ttl=64 time=0.129 ms
^C
--- a3 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.094/0.129/0.164 ms
/ # ping a4
ping: bad address 'a4'
/ # read escape sequence
[node1] (local) root@192.168.0.13 ~
$ docker attach a3
/ # ping a1
PING a1 (172.19.0.2): 56 data bytes
64 bytes from 172.19.0.2: seq=0 ttl=64 time=0.111 ms
64 bytes from 172.19.0.2: seq=1 ttl=64 time=0.123 ms
^C
--- a1 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.111/0.117/0.123 ms
/ # ping a4
ping: bad address 'a4'
/ # ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
64 bytes from 172.17.0.3: seq=0 ttl=64 time=0.153 ms
64 bytes from 172.17.0.3: seq=1 ttl=64 time=0.092 ms
^C
--- 172.17.0.3 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.092/0.122/0.153 ms
/ # ping 172.17.0.4
PING 172.17.0.4 (172.17.0.4): 56 data bytes
^C
--- 172.17.0.4 ping statistics ---
7 packets transmitted, 0 packets received, 100% packet loss
/ # ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2): 56 data bytes
64 bytes from 172.17.0.2: seq=0 ttl=64 time=0.124 ms
64 bytes from 172.17.0.2: seq=1 ttl=64 time=0.084 ms
64 bytes from 172.17.0.2: seq=2 ttl=64 time=0.069 ms
64 bytes from 172.17.0.2: seq=3 ttl=64 time=0.070 ms
^C
--- 172.17.0.2 ping statistics ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 0.069/0.086/0.124 ms
/ # ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: seq=0 ttl=51 time=1.197 ms
64 bytes from 8.8.8.8: seq=1 ttl=51 time=1.165 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 1.165/1.181/1.197 ms
/ # read escape sequence

[node1] (local) root@192.168.0.13 ~
$ docker attach a1
/ # ping 8.8.8.8.
ping: bad address '8.8.8.8.'
/ # ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: seq=0 ttl=51 time=1.118 ms
64 bytes from 8.8.8.8: seq=1 ttl=51 time=1.182 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 1.118/1.150/1.182 ms
/ # read escape sequence
[node1] (local) root@192.168.0.13 ~
$ docker attach a4
/ # ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: seq=0 ttl=51 time=1.296 ms
64 bytes from 8.8.8.8: seq=1 ttl=51 time=1.193 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 1.193/1.244/1.296 ms
/ # read escape sequence


[node1] (local) root@192.168.0.13 ~
$ docker container stop a1 a2 a3 a4
a1
a2
a3
a4
[node1] (local) root@192.168.0.13 ~
$ docker container rm a1 a2 a3 a4
a1
a2
a3
a4
[node1] (local) root@192.168.0.13 ~
$ docker network rm a-net
a-net

[node1] (local) root@192.168.0.13 ~
$ docker network rm bridge
Error response from daemon: bridge is a pre-defined network and cannot be removed

```
