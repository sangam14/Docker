# Building a Flask Application with a Redis Database

All necessary files are present in the same folder.

### Using docker-compose build 
Optional - we can directly use docker-compose up command too.
When we have a Dockerfile, we can use this command to build the image.

```
$ docker-compose build
redis uses an image, skipping
Building app
Step 1/7 : FROM python:3.7.0-alpine3.8
3.7.0-alpine3.8: Pulling from library/python
4fe2ade4980c: Pull complete
7cf6a1d62200: Pull complete
b66090d9998c: Pull complete
e0dc374a57ff: Pull complete
eb580e903ff2: Pull complete
Digest: sha256:e12594db7297ebf9d9478ba60373e0181974f373016e7495926a7da81bda323b
Status: Downloaded newer image for python:3.7.0-alpine3.8
 ---> cf41883b24b8
Step 2/7 : WORKDIR /usr/src/app
 ---> Running in f9129505dc4d
Removing intermediate container f9129505dc4d
 ---> ea78c5aa4566
Step 3/7 : COPY requirements.txt ./
 ---> 7bdd785ffedc
Step 4/7 : RUN pip install --no-cache-dir -r requirements.txt
 ---> Running in 508d45689aed
Collecting flask (from -r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/9a/74/670ae9737d14114753b8c8fdf2e8bd212a05d3b361ab15b44937dfd40985/Flask-1.0.3-py2.py3-none-any.whl (92kB)
Collecting redis (from -r requirements.txt (line 2))
  Downloading https://files.pythonhosted.org/packages/ac/a7/cff10cc5f1180834a3ed564d148fb4329c989cbb1f2e196fc9a10fa07072/redis-3.2.1-py2.py3-none-any.whl (65kB)
Collecting Jinja2>=2.10 (from flask->-r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/1d/e7/fd8b501e7a6dfe492a433deb7b9d833d39ca74916fa8bc63dd1a4947a671/Jinja2-2.10.1-py2.py3-none-any.whl (124kB)
Collecting itsdangerous>=0.24 (from flask->-r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/76/ae/44b03b253d6fade317f32c24d100b3b35c2239807046a4c953c7b89fa49e/itsdangerous-1.1.0-py2.py3-none-any.whl
Collecting click>=5.1 (from flask->-r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/fa/37/45185cb5abbc30d7257104c434fe0b07e5a195a6847506c074527aa599ec/Click-7.0-py2.py3-none-any.whl (81kB)
Collecting Werkzeug>=0.14 (from flask->-r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/9f/57/92a497e38161ce40606c27a86759c6b92dd34fcdb33f64171ec559257c02/Werkzeug-0.15.4-py2.py3-none-any.whl (327kB)
Collecting MarkupSafe>=0.23 (from Jinja2>=2.10->flask->-r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/b9/2e/64db92e53b86efccfaea71321f597fa2e1b2bd3853d8ce658568f7a13094/MarkupSafe-1.1.1.tar.gz
Installing collected packages: MarkupSafe, Jinja2, itsdangerous, click, Werkzeug, flask, redis
  Running setup.py install for MarkupSafe: started
    Running setup.py install for MarkupSafe: finished with status 'done'
Successfully installed Jinja2-2.10.1 MarkupSafe-1.1.1 Werkzeug-0.15.4 click-7.0 flask-1.0.3 itsdangerous-1.1.0 redis-3.2.1
You are using pip version 18.1, however version 19.1.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
Removing intermediate container 508d45689aed
 ---> ec5edcd6aa3b
Step 5/7 : COPY . .
 ---> 5e503f38f22b
Step 6/7 : ENV FLASK_APP=app.py
 ---> Running in 8607f291422f
Removing intermediate container 8607f291422f
 ---> ccf7a5e19f35
Step 7/7 : CMD flask run --host=0.0.0.0
 ---> Running in 5ef4038810ee
Removing intermediate container 5ef4038810ee
 ---> 669faab7e214
Successfully built 669faab7e214
Successfully tagged flask-redis:1.0

```

```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
flask-redis         1.0                 669faab7e214        20 minutes ago      88.3MB
python              3.7.0-alpine3.8     cf41883b24b8        8 months ago        78.1MB
```

### Runnning the Application

```
$ docker-compose up -d
Creating network "flask-redis_default" with the default driver
Pulling redis (redis:4.0.11-alpine)...
4.0.11-alpine: Pulling from library/redis
4fe2ade4980c: Already exists
fb758dc2e038: Pull complete
989f7b0c858b: Pull complete
d5318f13abaa: Pull complete
3521559474dd: Pull complete
add04b113886: Pull complete
Creating flask-redis_redis_1 ... done
Creating flask-redis_app_1   ... done



[node2] (local) root@192.168.0.12 /test/Docker/Docker Compose/flask-redis
$ docker-compose ps
       Name                      Command               State           Ports
-------------------------------------------------------------------------------------
flask-redis_app_1     /bin/sh -c flask run --hos ...   Up      0.0.0.0:5000->5000/tcp
flask-redis_redis_1   docker-entrypoint.sh redis ...   Up      6379/tcp

```

### Testing the Application

1) Posting Data
```
$ curl --header "Content-Type: application/json" \
> --request POST \
> --data '{"name":"Prashansa"}' localhost:5000

{
  "name": "Prashansa"
}
```

2) Retrieving data

```
$ curl localhost:5000
[
  "\\{'name':Prashansa\\}"
]
```

### Network Connectivity between Containers

```
$ docker network ls
NETWORK ID          NAME                        DRIVER              SCOPE
344ec39bd0ca        bridge                      bridge              local
aa7019865c83        flask-redis_default         bridge              local
8cea80409ff9        host                        host                local
dfd444fd66e5        none                        null                local
3f5442234fbd        wordpress-service_default   bridge              local
```

```
$ docker network inspect flask-redis_default
[
    {
        "Name": "flask-redis_default",
        "Id": "aa7019865c83f7b29728e7819906d14c2a48929c735245e4fd726353a7576225",
        "Created": "2019-06-11T09:24:41.672287478Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.27.0.0/16",
                    "Gateway": "172.27.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": true,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "42b9c0a8a397d15711305087e5e368b85648dea63098249ac7fff202eba1cadf": {
                "Name": "flask-redis_app_1",
                "EndpointID": "4b84d57e7c279fbaa59ac3a62447825892562c824f639dc9c2fd060ec2b749d9",
                "MacAddress": "02:42:ac:1b:00:03",
                "IPv4Address": "172.27.0.3/16",
                "IPv6Address": ""
            },
            "a4802d2e919bca73f2eb6d38d0f9235b22d27437d36e97289fb829adfd30881b": {
                "Name": "flask-redis_redis_1",
                "EndpointID": "08a48ca12f7818b642162d6567ceb0fcf3999bfeebae91904bb4d7b71dc9b4f2",
                "MacAddress": "02:42:ac:1b:00:02",
                "IPv4Address": "172.27.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {
            "com.docker.compose.network": "default",
            "com.docker.compose.project": "flask-redis",
            "com.docker.compose.version": "1.23.2"
        }
    }
]
```

## Rebuilding

Changes are done in Dockerfile and docker-compose.yml
The changed files are Dockerfile(2) and docker-compose(2).yml


