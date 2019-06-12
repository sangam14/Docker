## For Swarm


```
$ docker swarm init --advertise-addr 192.168.0.13
Swarm initialized: current node (oxw4xzhv8bbdmiyutq9h97ibx) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-5v20a56yuipogbkkqthajw088okp42v3tr3rz3vq7bgs54toe3-52wl722gc3d81w2n4x1hndzty 192.168.0.13:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

[node1] (local) root@192.168.0.13 ~
$ docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
oxw4xzhv8bbdmiyutq9h97ibx *   node1               Ready               Active              Leader              19.03.0-beta2
yeleaom4ybi2h29za0fzlkjtb     node2               Ready               Active                                  19.03.0-beta2
39fi8exf64m839kentj7s7baq     node3               Ready               Active                                  19.03.0-beta2
[node1] (local) root@192.168.0.13 ~
$ docker node ls --filter role=manager
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
oxw4xzhv8bbdmiyutq9h97ibx *   node1               Ready               Active              Leader              19.03.0-beta2
[node1] (local) root@192.168.0.13 ~
$ docker node ls --filter role=worker
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
yeleaom4ybi2h29za0fzlkjtb     node2               Ready               Active                                  19.03.0-beta2
39fi8exf64m839kentj7s7baq     node3               Ready               Active                                  19.03.0-beta2
[node1] (local) root@192.168.0.13 ~



[node1] (local) root@192.168.0.13 ~
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
43cd6abbf133        bridge              bridge              local
f9ca8020472b        docker_gwbridge     bridge              local
d891c58315c0        host                host                local
ma3q7hn4r84o        ingress             overlay             swarm
17072497635b        none                null                local


[node1] (local) root@192.168.0.13 ~
$ docker network create -d overlay nginx-net
e6wpr3n5f4d4pdtubb5odrcz3
[node1] (local) root@192.168.0.13 ~
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
43cd6abbf133        bridge              bridge              local
f9ca8020472b        docker_gwbridge     bridge              local
d891c58315c0        host                host                local
ma3q7hn4r84o        ingress             overlay             swarm
e6wpr3n5f4d4        nginx-net           overlay             swarm
17072497635b        none                null                local

[node1] (local) root@192.168.0.13 ~
$ docker service create --name nginx --network nginx-net --publish 80:80 --replicas=5  nginx
qy9vsna9szyv1fo96p8ufiww5
overall progress: 5 out of 5 tasks
1/5: running   [==================================================>]
2/5: running   [==================================================>]
3/5: running   [==================================================>]
4/5: running   [==================================================>]
5/5: running   [==================================================>]
verify: Service converged


node1] (local) root@192.168.0.13 ~
$ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
qy9vsna9szyv        nginx               replicated          5/5                 nginx:latest        *:80->80/tcp
[node1] (local) root@192.168.0.13 ~
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS  NAMES
bbf4dfa1fdbc        nginx:latest        "nginx -g 'daemon of…"   2 minutes ago       Up 2 minutes        80/tcp  nginx.2.qe2favg4soag9hfc1ys3y4pwy
85b377c6c014        nginx:latest        "nginx -g 'daemon of…"   2 minutes ago       Up 2 minutes        80/tcp  nginx.4.qbzjy9pc1ulsurhd6wgbb3ccg
3f18dac3bb7d        nginx               "nginx -g 'daemon of…"   21 minutes ago      Up 21 minutes  my_nginx
[node1] (local) root@192.168.0.13 ~

[node1] (local) root@192.168.0.13 ~
$ docker network inspect nginx-net
[
    {
        "Name": "nginx-net",
        "Id": "e6wpr3n5f4d4pdtubb5odrcz3",
        "Created": "2019-06-12T11:50:49.90441194Z",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "10.0.0.0/24",
                    "Gateway": "10.0.0.1"
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
            "85b377c6c014179cab9779a5bc654ad05e69de9163615cf2400bf558ca84b3e2": {
                "Name": "nginx.4.qbzjy9pc1ulsurhd6wgbb3ccg",
                "EndpointID": "336574bd6c44bec8ae2d118fdb9253e4fa985450fa57bbef3f0ff083dca78bcd",
                "MacAddress": "02:42:0a:00:00:06",
                "IPv4Address": "10.0.0.6/24",
                "IPv6Address": ""
            },
            "bbf4dfa1fdbc63ac17e1e434214a275c593663d71a0d98d907dced5259321f69": {
                "Name": "nginx.2.qe2favg4soag9hfc1ys3y4pwy",
                "EndpointID": "7657dfe37e9de9bc6d2ee8111e697a0ca910837fb8cb167c7f4b99874acda91e",
                "MacAddress": "02:42:0a:00:00:04",
                "IPv4Address": "10.0.0.4/24",
                "IPv6Address": ""
            },
            "lb-nginx-net": {
                "Name": "nginx-net-endpoint",
                "EndpointID": "1ef5c0f5324f5df0124fdc836dcbe2d315dec9fe46a51faac1c3ee0f3d3801a5",
                "MacAddress": "02:42:0a:00:00:08",
                "IPv4Address": "10.0.0.8/24",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.driver.overlay.vxlanid_list": "4097"
        },
        "Labels": {},
        "Peers": [
            {
                "Name": "dba6fe14d3b6",
                "IP": "192.168.0.13"
            },
            {
                "Name": "122bfd6a19fc",
                "IP": "192.168.0.12"
            },
            {
                "Name": "989f9fdb217e",
                "IP": "192.168.0.11"
            }
        ]
    }
]
------- below one from worker

[node1] (local) root@192.168.0.12 ~
$


$ docker network inspect nginx-net
[
    {
        "Name": "nginx-net",
        "Id": "e6wpr3n5f4d4pdtubb5odrcz3",
        "Created": "2019-06-12T11:50:49.903172839Z",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "10.0.0.0/24",
                    "Gateway": "10.0.0.1"
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
            "4daa45694b492d482617795c9b5e291f19ae6fea0cb70ef1559ba07e8c24b47c": {
                "Name": "nginx.5.p03hinlaoozo485l5apuojiph",
                "EndpointID": "1071667097f7e233fa7ac5727dab50245dbbcd55dc2aa41cdaebef967b8edde6",
                "MacAddress": "02:42:0a:00:00:07",
                "IPv4Address": "10.0.0.7/24",
                "IPv6Address": ""
            },
            "764a469a0bd40b370fd72b34d9784ea4856e2d82c944d94f9d6407bf6b9652df": {
                "Name": "nginx.3.m0rbweyuzer9jkgj66o20pi7a",
                "EndpointID": "0872138a834e7d35284f0d65c11601cb39baa473c1b2fe6806ad372e01f2e459",
                "MacAddress": "02:42:0a:00:00:05",
                "IPv4Address": "10.0.0.5/24",
                "IPv6Address": ""
            },
            "lb-nginx-net": {
                "Name": "nginx-net-endpoint",
                "EndpointID": "5731e8e3ac818928c7689ab5af7b726bc07e5bf5cf2941aef847ed5c5fd964d1",
                "MacAddress": "02:42:0a:00:00:09",
                "IPv4Address": "10.0.0.9/24",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.driver.overlay.vxlanid_list": "4097"
        },
        "Labels": {},
        "Peers": [
            {
                "Name": "122bfd6a19fc",
                "IP": "192.168.0.12"
            },
            {
                "Name": "989f9fdb217e",
                "IP": "192.168.0.11"
            },
            {
                "Name": "dba6fe14d3b6",
                "IP": "192.168.0.13"
            }
        ]
    }
]
----

[node1] (local) root@192.168.0.13 ~
$ docker network create -d overlay new-nginx
aor5c1ly5xo3i5a4g9mplmb6c

[node1] (local) root@192.168.0.13 ~
$ docker service update --network-add new-nginx nginx
nginx
overall progress: 5 out of 5 tasks
1/5: running   [==================================================>]
2/5: running   [==================================================>]
3/5: running   [==================================================>]
4/5: running   [==================================================>]
5/5: running   [==================================================>]
verify: Service converged

[node1] (local) root@192.168.0.13 ~
$ docker service inspect nginx
[
    {
        "ID": "qy9vsna9szyv1fo96p8ufiww5",
        "Version": {
            "Index": 94
        },
        "CreatedAt": "2019-06-12T11:50:49.715313074Z",
        "UpdatedAt": "2019-06-12T12:01:16.483866313Z",
        "Spec": {
            "Name": "nginx",
            "Labels": {},
            "TaskTemplate": {
                "ContainerSpec": {
                    "Image": "nginx:latest@sha256:bdbf36b7f1f77ffe7bd2a32e59235dff6ecf131e3b6b5b96061c652f30685f3a",
                    "Init": false,
                    "StopGracePeriod": 10000000000,
                    "DNSConfig": {},
                    "Isolation": "default"
                },
                "Resources": {
                    "Limits": {},
                    "Reservations": {}
                },
                "RestartPolicy": {
                    "Condition": "any",
                    "Delay": 5000000000,
                    "MaxAttempts": 0
                },
                "Placement": {
                    "Platforms": [
                        {
                            "Architecture": "amd64",
                            "OS": "linux"
                        },
                        {
                            "OS": "linux"
                        },
                        {
                            "Architecture": "arm64",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "386",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "ppc64le",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "s390x",
                            "OS": "linux"
                        }
                    ]
                },
                "Networks": [
                    {
                        "Target": "aor5c1ly5xo3i5a4g9mplmb6c"
                    },
                    {
                        "Target": "e6wpr3n5f4d4pdtubb5odrcz3"
                    }
                ],
                "ForceUpdate": 0,
                "Runtime": "container"
            },
            "Mode": {
                "Replicated": {
                    "Replicas": 5
                }
            },
            "UpdateConfig": {
                "Parallelism": 1,
                "FailureAction": "pause",
                "Monitor": 5000000000,
                "MaxFailureRatio": 0,
                "Order": "stop-first"
            },
            "RollbackConfig": {
                "Parallelism": 1,
                "FailureAction": "pause",
                "Monitor": 5000000000,
                "MaxFailureRatio": 0,
                "Order": "stop-first"
            },
            "EndpointSpec": {
                "Mode": "vip",
                "Ports": [
                    {
                        "Protocol": "tcp",
                        "TargetPort": 80,
                        "PublishedPort": 80,
                        "PublishMode": "ingress"
                    }
                ]
            }
        },
        "PreviousSpec": {
            "Name": "nginx",
            "Labels": {},
            "TaskTemplate": {
                "ContainerSpec": {
                    "Image": "nginx:latest@sha256:bdbf36b7f1f77ffe7bd2a32e59235dff6ecf131e3b6b5b96061c652f30685f3a",
                    "Init": false,
                    "DNSConfig": {},
                    "Isolation": "default"
                },
                "Resources": {
                    "Limits": {},
                    "Reservations": {}
                },
                "Placement": {
                    "Platforms": [
                        {
                            "Architecture": "amd64",
                            "OS": "linux"
                        },
                        {
                            "OS": "linux"
                        },
                        {
                            "Architecture": "arm64",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "386",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "ppc64le",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "s390x",
                            "OS": "linux"
                        }
                    ]
                },
                "Networks": [
                    {
                        "Target": "e6wpr3n5f4d4pdtubb5odrcz3"
                    }
                ],
                "ForceUpdate": 0,
                "Runtime": "container"
            },
            "Mode": {
                "Replicated": {
                    "Replicas": 5
                }
            },
            "EndpointSpec": {
                "Mode": "vip",
                "Ports": [
                    {
                        "Protocol": "tcp",
                        "TargetPort": 80,
                        "PublishedPort": 80,
                        "PublishMode": "ingress"
                    }
                ]
            }
        },
        "Endpoint": {
            "Spec": {
                "Mode": "vip",
                "Ports": [
                    {
                        "Protocol": "tcp",
                        "TargetPort": 80,
                        "PublishedPort": 80,
                        "PublishMode": "ingress"
                    }
                ]
            },
            "Ports": [
                {
                    "Protocol": "tcp",
                    "TargetPort": 80,
                    "PublishedPort": 80,
                    "PublishMode": "ingress"
                }
            ],
            "VirtualIPs": [
                {
                    "NetworkID": "ma3q7hn4r84o2hn3x77u8qvfe",
                    "Addr": "10.255.0.5/16"
                },
                {
                    "NetworkID": "e6wpr3n5f4d4pdtubb5odrcz3",
                    "Addr": "10.0.0.2/24"
                },
                {
                    "NetworkID": "aor5c1ly5xo3i5a4g9mplmb6c",
                    "Addr": "10.0.1.2/24"
                }
            ]
        },
        "UpdateStatus": {
            "State": "completed",
            "StartedAt": "2019-06-12T12:00:38.236988784Z",
            "CompletedAt": "2019-06-12T12:01:16.483827413Z",
            "Message": "update completed"
        }
    }
]


[node1] (local) root@192.168.0.13 ~
$ docker service update --network-rm new-nginx nginx
nginx
overall progress: 5 out of 5 tasks
1/5: running   [==================================================>]
2/5: running   [==================================================>]
3/5: running   [==================================================>]
4/5: running   [==================================================>]
5/5: running   [==================================================>]
verify: Service converged


[node1] (local) root@192.168.0.13 ~
$ docker service rm nginx
nginx
[node1] (local) root@192.168.0.13 ~
$ docker network rm n
new-nginx  nginx-net
[node1] (local) root@192.168.0.13 ~
$ docker network rm nginx-net
nginx-net
[node1] (local) root@192.168.0.13 ~
$ docker network rm new-nginx
new-nginx
```

## For Standalone Containers

```
[node1] (local) root@192.168.0.13 ~
$ docker network create -d overlay test
bph93zm4ovurn157wouhgv5bg

[node1] (local) root@192.168.0.13 ~
$ docker run -dit --name a1 alpine
3213ffc34e1fb930661ef114e1fe7c493df8fa641950c22203227a25c888a9cc

[node1] (local) root@192.168.0.13 ~
$ docker network connect test a1
Error response from daemon: Could not attach to network test: rpc error: code = PermissionDenied desc = network test not ma
nually attachable
[node1] (local) root@192.168.0.13 ~
$ docker run -dit --name a2 --network test alpine
fb2b474191c921151ac08f85eb4a8ae10107c5c509e79b24fcd2334201202b9d
docker: Error response from daemon: Could not attach to network test: rpc error: code = PermissionDenied desc = network test not manually attachable.
[node1] (local) root@192.168.0.13 ~
$ docker stop a1 a2
a1
a2
[node1] (local) root@192.168.0.13 ~
$ docker rm a1 a2
a1
a2
[node1] (local) root@192.168.0.13 ~
$ docker network rm test
test


Host 1:

[node1] (local) root@192.168.0.13 ~
$ docker network create -d overlay --attachable test-net
arpy93vyxrr70hv6ia1440vty
[node1] (local) root@192.168.0.13 ~
$ docker run -it --name alpine1 --network test-net alpine
/ #

Host 2:

[node2] (local) root@192.168.0.12 ~
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
7f2faab54d52        bridge              bridge              local
ccbdac706bbc        docker_gwbridge     bridge              local
dea0aa833d9b        host                host                local
ma3q7hn4r84o        ingress             overlay             swarm
065bd256b9c6        none                null                local
[node2] (local) root@192.168.0.12 ~


[node2] (local) root@192.168.0.12 ~
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
7f2faab54d52        bridge              bridge              local
ccbdac706bbc        docker_gwbridge     bridge              local
dea0aa833d9b        host                host                local
ma3q7hn4r84o        ingress             overlay             swarm
065bd256b9c6        none                null                local
arpy93vyxrr7        test-net            overlay             swarm


Host 1:

[node1] (local) root@192.168.0.13 ~
$ docker attach alpine1
/ # ping alpine2
PING alpine2 (10.0.3.4): 56 data bytes
64 bytes from 10.0.3.4: seq=0 ttl=64 time=0.286 ms
64 bytes from 10.0.3.4: seq=1 ttl=64 time=0.158 ms
64 bytes from 10.0.3.4: seq=2 ttl=64 time=0.212 ms
64 bytes from 10.0.3.4: seq=3 ttl=64 time=0.159 ms
64 bytes from 10.0.3.4: seq=4 ttl=64 time=0.267 ms

[node1] (local) root@192.168.0.13 ~
$ docker stop alpine1
alpine1
[node1] (local) root@192.168.0.13 ~
$ docker rm alpine1
alpine1
[node1] (local) root@192.168.0.13 ~

[node1] (local) root@192.168.0.13 ~
$ docker network rm test-net
Error response from daemon: rpc error: code = FailedPrecondition desc = network arpy93vyxrr70hv6ia1440vty is in use by task
 hm7jshtlypgnum9i8118li0lh
[node1] (local) root@192.168.0.13 ~

Host 2:

[node2] (local) root@192.168.0.12 ~
$ docker stop alpine2
alpine2
[node2] (local) root@192.168.0.12 ~
$ docker rm alpine2
alpine2
[node2] (local) root@192.168.0.12 ~


Host 1:
[node1] (local) root@192.168.0.13 ~
$ docker network rm test-net
test-net



```





