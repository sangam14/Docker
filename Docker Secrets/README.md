# Managing Sensitive Data

Let's check a simple example of a docker-compose.yml file:

```
version: "3.2"

services:
 db:
  image: mysql:5.7
  restart: always
  environment:
   MYSQL_ROOT_PASSWORD: db
   MYSQL_DATABASE: wordpress
 wordpress:
  image: wordpress:latest
  restart: always
  ports:
   - "8080:80"
  environment:
   WORDPRESS_DB_HOST: db:3306
   WORDPRESS_DB_NAME: wordpress
   WORDPRESS_DB_USER: root
   WORDPRESS_DB_PASSWORD: db
  depends_on:
   - db
   
```

As it can be seen, that sensitive content like username and password for the database are being passed in clear text in the file. This is a security vulnerability and can be misused for malicious purposes. 

A critical element of maintaining the security of applications is using of a secure way of communicating sensitive data between containers like usernames, passwords, security certificates, tokens, etc. Such information is referred to as 'application secrets'. 

## Docker Secrets

For ensuring Trusted Delivery of application secrets, the concept of Docker Secrets is used. 
Docker Secrets follow the following fundamental requirements of security best practices:
1) Encrypting the secrets while they are in transit or at rest.
2) Preventing leakage of secret while the application is consuming it.
3) Adhering to the principle of least privilege - an application has access to only the secret that it needs.

As per the official documentation,
You can use secrets to manage any sensitive data which a container needs at runtime but you don’t want to store in the image or in source control, such as:
1) Usernames and passwords
2) TLS certificates and keys
3) SSH keys
4) Other important data such as the name of a database or internal server
5) Generic strings or binary content (up to 500 kb in size)

Docker Secrets are available only in Swarm mode. So, enable swarm mode by running the following command:

```
$ docker swarm init

OR

$ docker swarm init --advertise-addr IP-address-of-node
```

When you create a swarm by running docker swarm init, Docker designates itself as a manager node. By default, the manager node generates a new root Certificate Authority (CA) along with a key pair, which are used to secure communications with other nodes that join the swarm.

### Creating a Secret

```
$echo "root" | docker secret create db_user -

$echo "db" | docker secret create db_password -

$echo "wordpress" | docker secret create db_name -
```
![Image1]()

### To view secrets

```
$ docker secret ls
```

![Image2]()

### Adding the secrets to the compose file

```
node1] (local) root@192.168.0.33 ~
$ echo "wordpress" | docker secret create db_name -
u08gdncxjlcvvqrornef1tpw1
[node1] (local) root@192.168.0.33 ~
$ echo "root" | docker secret create db_user -
rbqtivuuhyw0evt4m4diy0irq
[node1] (local) root@192.168.0.33 ~
$ echo "db" | docker secret create db_password -
48p8qdk4eb2c3wsmupv2716zp

[node1] (local) root@192.168.0.33 ~
$ docker-compose up -d
WARNING: Service "db" uses secret "db_password" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "db" uses secret "db_name" which is external. External secrets are not available to containers created bydocker-compose.
WARNING: Service "wordpress" uses secret "db_name" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "wordpress" uses secret "db_user" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "wordpress" uses secret "db_password" which is external. External secrets are not available to containerscreated by docker-compose.
WARNING: The Docker Engine you're using is running in swarm mode.

Compose does not use swarm mode to deploy services to multiple nodes in a swarm. All containers will be scheduled on the current node.

To deploy your application across the swarm, use `docker stack deploy`.

Creating network "root_default" with the default driver
Creating root_db_1 ... done
Creating root_wordpress_1 ... done
[node1] (local) root@192.168.0.33 ~
$ docker-compose ps
WARNING: Service "db" uses secret "db_password" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "db" uses secret "db_name" which is external. External secrets are not available to containers created bydocker-compose.
WARNING: Service "wordpress" uses secret "db_name" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "wordpress" uses secret "db_user" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "wordpress" uses secret "db_password" which is external. External secrets are not available to containerscreated by docker-compose.
      Name                    Command                 State      Ports
----------------------------------------------------------------------
root_db_1          docker-entrypoint.sh mysqld      Restarting
root_wordpress_1   docker-entrypoint.sh apach ...   Restarting
[node1] (local) root@192.168.0.33 ~
$ docker-compose down
WARNING: Service "db" uses secret "db_password" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "db" uses secret "db_name" which is external. External secrets are not available to containers created bydocker-compose.
WARNING: Service "wordpress" uses secret "db_name" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "wordpress" uses secret "db_user" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "wordpress" uses secret "db_password" which is external. External secrets are not available to containerscreated by docker-compose.
Stopping root_wordpress_1 ... done
Stopping root_db_1        ... done
Removing root_wordpress_1 ... done
Removing root_db_1        ... done
Removing network root_default
[node1] (local) root@192.168.0.33 ~
$ docker stack deploy --help

Usage:  docker stack deploy [OPTIONS] STACK

Deploy a new stack or update an existing stack

Aliases:
  deploy, up

Options:
      --bundle-file string     Path to a Distributed Application Bundle file
  -c, --compose-file strings   Path to a Compose file, or "-" to read from stdin
      --orchestrator string    Orchestrator to use (swarm|kubernetes|all)
      --prune                  Prune services that are no longer referenced
      --resolve-image string   Query the registry to resolve image digest and supported platforms
                               ("always"|"changed"|"never") (default "always")
      --with-registry-auth     Send registry authentication details to Swarm agents
[node1] (local) root@192.168.0.33 ~
$ docker stack deploy -c docker-compose.yml wordpress
Ignoring unsupported options: restart

Creating network wordpress_default
Creating service wordpress_wordpress
Creating service wordpress_db
[node1] (local) root@192.168.0.33 ~
$ docker-compose ps
WARNING: Service "db" uses secret "db_password" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "db" uses secret "db_name" which is external. External secrets are not available to containers created bydocker-compose.
WARNING: Service "wordpress" uses secret "db_name" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "wordpress" uses secret "db_user" which is external. External secrets are not available to containers created by docker-compose.
WARNING: Service "wordpress" uses secret "db_password" which is external. External secrets are not available to containerscreated by docker-compose.
Name   Command   State   Ports
------------------------------
[node1] (local) root@192.168.0.33 ~
$ docker service ls
ID                  NAME                  MODE                REPLICAS            IMAGE               PORTS
m7qn298caq11        wordpress_db          replicated          1/1                 mysql:5.7
4p8rud3pkezh        wordpress_wordpress   replicated          1/1                 wordpress:latest    *:8080->80/tcp
[node1] (local) root@192.168.0.33 ~
```







## Contributor

[Prashansa Kulshrestha](https://github.com/Prashansa-K)

