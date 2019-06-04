# What is a Dockerfile?
A Dockerfile is a text file which contains a series of commands or instructions. These instructions are executed in the order in which they are written.
Execution of these instructions takes place on a base image. Hence, on building the Dockerfile, the successive actions form a new image from the base parent image.

To write a Dockerfile, we need to keep in mind the following keywords:

### FROM
This directive lets a user define a base image to initiate the build process. Any image can be provided here - including the ones a user creates by himself.
If the image specified is not found on the host, Docker attempts to search and download it from Docker Hub (public repository) or any other specified private repositary.

Usage: FROM image-name
```
FROM centos:latest
```

### ADD
This command takes two arguments -  a source and a destination. The files present on the source (may be localhost system or some remote URL) are copied to the destination specified path in the container file system. 

Usage: ADD (source directory from base system or URL) (destination filesystem of container)

```
ADD /hello /hello
```

### VOLUME
This command lets user mount a volume or directory present on base system on the container. Thus, it is like enabling access to the base's directory which is mounted and any changes made to it are visible to the container.

```
VOLUME [/a1 /a2 /a3]
```

### MAINTAINER
It is used to provide author's name to the image formed.

```
MAINTAINER Prashansa Kulshrestha
```

### ENV
It is used to specify environment variables in the form of key-value pairs.

```
ENV DISPLAY :0
```
### LABEL
It is used to provide metadata for an image and is provided as key-value pair.

```
LABEL version="1.0"
```
### RUN
This takes a command as an argument and runs it over the base image to form a new layer. 


Default shell used for a Linux container is /bin/sh. For a Windows container, cmd /s /c shell is used by default.

```
RUN yum install httpd
```

### CMD
The shell command specified with CMD becomes the default command which executes when a container initiates from an image.
If used in conjunction with ENTRYPOINT command, CMD provides default argument to the ENTRYPOINT command.

```
CMD echo 'Hello World'
```

### ENTRYPOINT
It sets the concrete default application that is used every time a container is created from an image. It configures a container that can run as an executable.

Let's begin with an example:

All work below is done on Play-With-Docker platform.

To begin, we will create a new directory.

```
[node1] (local) root@192.168.0.38 ~
$ mkdir /test

[node1] (local) root@192.168.0.38 ~
$ cd /test

[node1] (local) root@192.168.0.38 /test
$ pwd
/test
```

Now, open a file named 'Dockerfile' with a text editor

```
[node1] (local) root@192.168.0.38 /test
$ vi Dockerfile
```
## Writing a Dockerfile
#### Setting a Base Image using FROM keyword

```
FROM ubuntu
```

Thus, our image would start building taking base as Ubuntu.

#### Defining the Author (Optional) using MAINTAINER keyword

```
MAINTAINER Prashansa Kulshrestha
```

#### Running a commands on the base image to form new layers using RUN keyword

```
RUN apt-get update
RUN apt-get install python
```
Since, the base image was Ubuntu, we can run Ubuntu commands here. These commands above install python over Ubuntu.

#### Adding a simple Hello World printing python file to the container's file system using ADD command

```
ADD hello.py /home/hello.py
ADD a.py /home/a.py
```

We will place our hello.py and a.py files in the newly created directory itself (/test). ADD command would copy it from /test (current working directory) of host system
to container's filesystem at /home. The destination directories in the container would be create incase they don't exist.

Code for hello.py:
```
print ("Hello World")
```

Code for a.py:
```
print ("Overriden Hello")
```

#### Specifying default execution environment for the container using CMD and ENTRYPOINT
These keywords let us define the default execution environment for a container when it just initiates from an image or just starts.
If a command is specified with CMD keyword, it is the first command which a container executes as soon as it instantiates from an image. However, command and arguments provided with CMD can be overridden if user specifies his own commands while running the container using 'docker run' command.'

ENTRYPOINT helps to create a executable container and the commands and arguments provided with this keyword are not overridden.

We can also provide the default application environment using ENTRYPOINT and default arguments to be passed to it via CMD keyword. This can be done as follows:
```
CMD ["/home/hello.py"]
ENTRYPOINT ["python"]
```
So, default application mode of container would be python and if no other filename is provided as argument to it then it will execute hello.py placed in its /home directory.

Benefit of this is that user can choose some other file to run with the same application at runtime, that is, while launching the container.

So, our overall Dockerfile currently looks like this:

```
FROM ubuntu
MAINTAINER Prashansa Kulshrestha
RUN apt-get update
RUN apt-get install -y python
ADD hello.py /home/hello.py
ADD a.py /home/a.py
CMD ["/home/hello.py"]
ENTRYPOINT ["python"]
```

## Building a Dockerfile

To create an image from the Dockerfile, we need to build it. This is done as follows:

```
[node1] (local) root@192.168.0.38 /test
$ docker build -t pythonimage .
```

The option -t lets us tag our image with a name we desire. So, here we have named our image as 'pythonimage'.
The '.' in the end specifies current working directory i.e. /test. We initiated our build process from here. Docker would find the file named 'Dockerfile' in the current directory to process the build.

## Running a container from the newly built image

```
[node1] (local) root@192.168.0.38 /test
$ docker run --name test1 pythonimage
Hello World
[node1] (local) root@192.168.0.38 /test
$
```

So, here /home/hello.py file placed in the container executed and displayed the output 'Hello World', since it was specified as default with CMD keyword.

```
[node1] (local) root@192.168.0.38 /test
$ docker run --name test2 pythonimage /home/a.py
Overriden Hello 
[node1] (local) root@192.168.0.38 /test
$
```
Here, user specified another file to be run with python (default application for this container). So, the file specified with CMD got overridden and we obtained the output from /home/a.py.










