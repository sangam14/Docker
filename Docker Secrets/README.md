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







## Contributor

[Prashansa Kulshrestha](https://github.com/Prashansa-K)

