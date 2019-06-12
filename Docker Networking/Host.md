
```
$ docker run --rm -d --network host --name my_nginx nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
fc7181108d40: Pull complete
c4277fc40ec2: Pull complete
780053e98559: Pull complete
Digest: sha256:bdbf36b7f1f77ffe7bd2a32e59235dff6ecf131e3b6b5b96061c652f30685f3a
Status: Downloaded newer image for nginx:latest
c342922b75db19dd909363762c071e2d3d3bc6777b6a2cfd8628236b5f40e05f

[node1] (local) root@192.168.0.13 ~
$ netstat -tnlp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.11:33869        0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      10529/nginx: master
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      27/sshd
tcp        0      0 :::2375                 :::*                    LISTEN      6/dockerd
tcp        0      0 :::22                   :::*                    LISTEN      27/sshd


$ curl localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>


[node1] (local) root@192.168.0.13 ~
$ docker stop my_nginx
my_nginx

[node1] (local) root@192.168.0.13 ~
$ curl localhost
curl: (7) Failed to connect to localhost port 80: Connection refused


[node1] (local) root@192.168.0.13 ~
$ docker run --rm -d --network host --name my_nginx nginx
3f18dac3bb7d94b0628814b1664734eef1a7dc249453fd574f8ac12ea4370f46
[node1] (local) root@192.168.0.13 ~
$ curl localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
[node1] (local) root@192.168.0.13 ~

```
