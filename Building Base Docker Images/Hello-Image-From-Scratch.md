03/06/2019


# Building Images From Scratch

For this, Play With Docker platform was utilised.

1) Make a new directory

```
node1] (local) root@192.168.0.27 ~
$ mkdir helloimage
[node1] (local) root@192.168.0.27 ~
$ cd helloimage
[node1] (local) root@192.168.0.27 ~/helloimage
$ pwd
/root/helloimage
```

2) Make a simple C++ file which prints helloworld - hello.cpp

```
#include<iostream>
using namespace std;
int main()
{
  cout<<"Hello from Container";
return 0;
}
```

3) Compile the file using g++ compiler with static flag.

```
$ g++ -o hello -static hello.cpp
[node1] (local) root@192.168.0.27 ~/helloimage
$ ls
hello      hello.cpp
```

This would create a binary executable named hello for us in the same directory

4) Make a Dockerfile

```
FROM scratch
ADD hello /
CMD ["/hello"]
```

'scratch' keyword is used when we are not using any parent image of any OS or other application for our Dockerfile. 
We are building a base image.

5) Build the file

```
$ docker build --tag helloimage .
Sending build context to Docker daemon  6.988MB
Step 1/3 : FROM scratch
 --->
Step 2/3 : ADD hello /
 ---> e434cd6776cf
Step 3/3 : CMD ["/hello"]
 ---> Running in 5107eda5b536
Removing intermediate container 5107eda5b536
 ---> b307e5c4880e
Successfully built b307e5c4880e
Successfully tagged helloimage:latest


[node1] (local) root@192.168.0.27 ~/helloimage
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
helloimage          latest              b307e5c4880e        4 seconds ago       6.98MB
```

6) Run a container from the newly built image

```
$ docker run --name test helloimage
Hello from Container

[node1] (local) root@192.168.0.27 ~/helloimage
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
66570370aa74        helloimage          "/hello"            6 seconds ago       Exited (0) 4 seconds ago                       test

```
